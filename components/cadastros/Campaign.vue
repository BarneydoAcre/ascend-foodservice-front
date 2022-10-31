<template>
    <v-dialog v-model="dialog" max-width="94vw" class="main-cadastros-customer">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="getCampaign">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Campanhas
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card max-height="85vh">
            <v-list>
                <v-list-item>
                    <AddCampaign @getCampaign="getCampaign" />
                </v-list-item>
            </v-list>
            <v-data-table dense fixed-header height="65vh" :headers="headers" :items="campaign" :items-per-page="-1" hide-default-footer>
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
import AddCampaign from "./actions/AddCampaign.vue";
export default {
    name: "Campaign",
    components: {
        AddCampaign
    },
    data () {
        return {
            dialog: false,
            headers: [
                { text: "ID", align: "center", justify: "center", value: "id"},
                { text: "Nome", align: "center", justify: "center", value: "name"},
                { text: "Tipo", align: "center", justify: "center", value: "type.name"},
                { text: "De", align: "center", justify: "center", value: "from"},
                { text: "Até", align: "center", justify: "center", value: "at"},
                { text: "Ações", align: "center", justify: "center", value: "actions"},
            ],
            campaign: [
                {
                    id: 1,
                    name: 'compre 10 leve 1',
                    type: {
                        id: 2,
                        name: 'LEVE/GANHE',
                    },
                    from: '01.11.2022',
                    at: '01.01.2023'
                }
            ]
        }
    },
    methods: {
        async getCampaign() {
            const req = await fetch(process.env.HOST_BACK+'/register/getCampaign/?'+new URLSearchParams({
                'company': localStorage.getItem("company"),
                'token': localStorage.getItem("refresh")
            }), {
                method: "GET",
            })
            
            if (req.status == 200) {
                const res = await req.json()
                this.campaign = res
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