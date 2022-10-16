<template>
    <div class="main-sale">
        <div style="grid-area: filter;">
            <v-card height="14vh">
                <v-card-text>
                    <FilterSales @filterSales="filterSales"></FilterSales>
                </v-card-text>
            </v-card>
        </div>
        <ListSales 
        :sales="filteredSales" 
        :saleItems="saleItems"></ListSales>
        <Actions></Actions>
    </div>
</template>

<script>
import ListSales from '../../components/vendas/ListSales.vue';
import FilterSales from '../../components/vendas/FilterSales.vue';
import Actions from '../../components/vendas/Actions.vue';
export default {
    name: "vendas",
    components: {
        ListSales,
        FilterSales,
        Actions
    },
    data () {
        return {
            sales: [],
            saleItems: [],
            filteredSales: [],
        }
    },
    mounted () {
        this.getSale()
        this.getSaleItems()
        this.filterSales()
    },
    methods: {
        async getSale() {
            const req = await fetch(process.env.HOST_BACK+'/sale/getSale/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                company_worker: localStorage.getItem('user_id'),
            }),{
                method: "GET",
            })
            const res = await req.json()

            if (req.status == 200) {
                this.sales = res
                this.filteredSales = this.sales
            }
        },
        async getSaleItems() {
            const req = await fetch(process.env.HOST_BACK+'/sale/getSaleItems/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                company_worker: localStorage.getItem('user_id'),
            }),{
                method: "GET",
            })
            const res = await req.json()

            if (req.status == 200) {
                this.saleItems = res
            }
        },
        filterSales () {
            let sales = this.sales
            const urlParams = new URLSearchParams(window.location.search)
            let date = urlParams.get('date')
            if (date == 'null' || date == null) {
                var dateStart = new Date('01.01.2001')
                var dateEnd = new Date()
            }else {
                var dateStart = new Date(urlParams.get('date').split(',')[0])
                var dateEnd = new Date(urlParams.get('date').split(',')[1])
            }
            this.filteredSales = sales.filter((i) => { 
                let date = new Date(i.date)
                let sale = urlParams.get('sale')
                if (sale != 'null' && sale != '' && sale != null) {
                    return (date >= dateStart && date <= dateEnd && i.id == sale)
                }else {
                    return (date >= dateStart && date <= dateEnd)                    
                }
            })
        }
    }
}
</script>

<style>
.main-sale {
    display: grid;
    grid-template-columns: 25% 25% 20% 20% 10%;
    grid-template-rows: 10vh 6vh 68vh 8vh;
    grid-template-areas: 'filter filter filter filter filter'
                         'filter filter filter filter filter'
                         'table table table table table'
                         'actions actions actions actions actions';
}
</style>
