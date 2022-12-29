<template>
    <v-col :cols="cols">
        <SearchText ref="paymentForm" :headers="[
            { text: 'ID', align: 'center', justify: 'center', value: 'id' },
            { text: 'Nome', value: 'name' },
            { text: 'Ações', value: 'actions' },
        ]" 
        :items="items" 
        :loading="loading" :mainLabel="'Formas de Pagamento'"
        :subLabel="'Forma de Pagamento'" @action="getPaymentForm" />
    </v-col>
  
</template>

<script>
import SearchText from '../../default/SearchText.vue';
export default {
    name: 'PaymentForm',
    props: ['cols'],
    components: {
        SearchText
    },
    data: () => ({
        items: [],
        loading: false
    }),
    methods: {
        async getPaymentForm () {
            this.loading = true
            const req = await fetch(process.env.HOST_BACK+'/register/getPaymentForm/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company')
            }), {
                method: 'GET'
            })
            if (req.status == 200) {
                const res = await req.json()
                this.items = res
                this.loading = false
            }
        },
    }
}
</script>

<style>

</style>