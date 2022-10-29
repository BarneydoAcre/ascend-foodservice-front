<template>
    <v-dialog v-model="dialog" max-width="94vw" class="main-cadastros-customer">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="getPartner">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Clientes
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card max-height="85vh">
            <v-list>
                <v-list-item>
                    <AddCustomer @getPartner="getPartner" />
                </v-list-item>
            </v-list>
            <v-data-table dense fixed-header height="65vh" :headers="headers" :items="customers" :items-per-page="-1" hide-default-footer>
                <template v-slot:item.actions="{ item }">
                    <v-icon 
                    small 
                    @click="console.log(item.id)">mdi-pencil</v-icon>
                    <v-icon 
                    small 
                    @click="console.log(item.id)">mdi-delete</v-icon>
                </template>
            </v-data-table>
        </v-card>
    </v-dialog>
</template>

<script>
import AddCustomer from "./actions/AddCustomer.vue";
export default {
    name: "Customer",
    components: {
        AddCustomer
    },
    data () {
        return {
            dialog: false,
            headers: [
                { text: "ID", align: "center", justify: "center", value: "id"},
                { text: "Nome/Razão", align: "center", justify: "center", value: "name"},
                { text: "Apelido/Fantasia", align: "center", justify: "center", value: "fantasy"},
                { text: "CPF/CNPJ", align: "center", justify: "center", value: "cpf_cnpj"},
                { text: "RG/IE", align: "center", justify: "center", value: "rg_ie"},
                { text: "Ações", align: "center", justify: "center", value: "actions"},
            ],
            customers: []
        }
    },
    methods: {
        async getPartner() {
            const req = await fetch(process.env.HOST_BACK+'/register/getPartner/?'+new URLSearchParams({
                'company': localStorage.getItem("company"),
                'token': localStorage.getItem("refresh")
            }), {
                method: "GET",
            })
            const res = await req.json()

            if (req.status == 200) {
                this.customers = res
            }
        }
    }
}
</script>

<style>
.main-foodservice-product {
    grid-area: customer;

    padding: 1vh 1vh 1vh 0;
}
</style>