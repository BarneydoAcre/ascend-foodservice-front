<template>
    <v-col :cols="cols">
        <SearchText ref="expense" :headers="[
            { text: 'ID', align: 'center', justify: 'center', value: 'id' },
            { text: 'Nome', value: 'name' },
            { text: 'Ações', value: 'actions' },
        ]" 
        :items="items" 
        :loading="loading" :mainLabel="'Despesas'"
        :subLabel="'Despesa'" @action="getExpense" />
    </v-col>
  
</template>

<script>
import SearchText from '../../default/SearchText.vue';
export default {
    name: 'Expense',
    props: ['cols'],
    components: {
        SearchText
    },
    data: () => ({
        items: [],
        loading: false
    }),
    methods: {
        async getExpense () {
            this.loading = true
            const req = await fetch(process.env.HOST_BACK+'/register/getExpense/?'+new URLSearchParams({
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