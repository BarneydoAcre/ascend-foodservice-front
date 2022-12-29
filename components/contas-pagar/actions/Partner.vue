<template>
    <v-col :cols="cols">
        <SearchText ref="partner" :headers="[
            { text: 'ID', align: 'center', justify: 'center', value: 'id' },
            { text: 'Nome', value: 'name' },
            { text: 'Ações', value: 'actions' },
        ]" 
        :items="items" 
        :loading="loading" :mainLabel="'Parceiros'"
        :subLabel="'Parceiro'" @action="getPartner" />
    </v-col>
  
</template>

<script>
import SearchText from '../../default/SearchText.vue';
export default {
    name: 'Partner',
    props: ['cols'],
    components: {
        SearchText
    },
    data: () => ({
        items: [],
        loading: false
    }),
    methods: {
        async getPartner () {
            this.loading = true
            const req = await fetch(process.env.HOST_BACK+'/register/getPartner/?'+new URLSearchParams({
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