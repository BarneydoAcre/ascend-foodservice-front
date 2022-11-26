<template>
    <v-dialog>
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="1">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Add Conta a Pagar
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card>
            <v-card-title>
                Nova Conta a Pagar
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validationc>
                    <v-row dense>
                        <v-col cols="2">
                            <DateField />
                        </v-col>
                        <v-col cols="5">
                            <SearchText :headers="headers" :items="items" 
                            :loading="loadingSelect" :mainLabel="'Parceiros'"
                            :subLabel="'Parceiro'"  
                            @getPartner="getPartner" />
                        </v-col>
                    </v-row>
                </v-form>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
import SelectPartner from './actions/SelectPartner.vue';
import DateField from '../default/DateField.vue';
import SearchText from '../default/SearchText.vue';
export default {
    name: 'NewPayment',
    data: () => ({
        valid: false,
        activePicker: null,
        loadingSelect: false,
        date: null,
        menu: false,
        items: [],
        headers: [
            { 
                text: "ID",
                align: "center",
                justify: "center",
                value: "id"
            },
            { text: "Nome", value: "name" },
            { text: "Ações", value: "actions" },
        ],
    }),
    components: {
        SearchText,
        SelectPartner,
        DateField
    },
    methods: {
        async getPartner () {
            this.loadingSelect = true
            const req = await fetch(process.env.HOST_BACK+'/register/getPartner/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company')
            }), {
                method: 'GET'
            })
            if (req.status == 200) {
                const res = await req.json()
                this.items = res
                this.loadingSelect = false
            }
        }
    }
}
</script>

<style>

</style>