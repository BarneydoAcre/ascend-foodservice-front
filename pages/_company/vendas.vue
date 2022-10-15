<template>
    <div class="main-sale">
        <ListSales :sales="sales" :saleItems="saleItems"></ListSales>
    </div>
</template>

<script>
import ListSales from '../../components/vendas/ListSales.vue';
export default {
    name: "vendas",
    components: { ListSales },
    data () {
        return {
            sales: [],
            saleItems: [],
        }
    },
    mounted () {
        this.getSale()
        this.getSaleItems()
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
    }
}
</script>

<style>
.main-sale {
    display: grid;
    grid-template-columns: 25% 25% 20% 20% 10%;
    grid-template-rows: 9vh 21.75vh 21.75vh 21.75vh 21.75vh ;
    grid-template-areas: 'actions actions graphs graphs graphs'
                         'product product none none none'
                         'table table table table table'
                         'table table table table table'
                         'table table table table table';
}
</style>
