<template>
    <v-dialog v-model="dialog" max-width="45vw">
        <template v-slot:activator="{ on, attrs }">
            <div 
            color="primary" 
            v-bind="attrs">
                <v-text-field dense filled :loading="loadingSelect" @click="getPartner" @keydown.enter="filterPartner" label="Parceiro" v-model="partner.name" :rules="rules"></v-text-field>
            </div>
        </template>
        <v-card>
            <v-card-title>Parceiros</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="12">
                            <v-text-field dense filled label="Digite o nome" v-model="partner.name" @keyup="filterPartner"></v-text-field>
                        </v-col>
                    </v-row>
                </v-form>
                <div style="max-height: 45vh; overflow-y: auto;">
                    <v-data-table
                    dense 
                    class="elevation-1" 
                    :headers="headers" 
                    :items-per-page="-1" 
                    :items="filtered" 
                    hide-default-footer>
                        <template v-slot:item.actions="{ item }">
                            <v-icon small @click="setItem(item.id)">mdi-plus</v-icon>
                        </template>
                    </v-data-table>
                </div>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: "SelectPartner",
    data () {
        return {
            loadingSelect: false,
            valid: false,
            partner: { id: null, name: null },
            quantity: null,
            partners: [],
            dialog: false,
            filtered: [],
            rules: [
                v => !!v || "Obrigatório",
            ],
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
        }
    },
    mounted () {
        this.getPartner()
    },
    methods: {
        reset () {
            this.$refs.form.reset();
        },
        async getPartner() {
            this.loadingSelect = true
            const req = await fetch(process.env.HOST_BACK+'/register/getPartner/?'+new URLSearchParams({
                'company': localStorage.getItem("company"),
                'token': localStorage.getItem("refresh")
            }), {
                method: "GET",
            })

            if (req.status == 200) {
                console.log(this.partner)
                const res = await req.json()
                this.partners = res
                this.loadingSelect = false
            }
        },
        filterPartner () {
            if(this.partner.name == null || this.partner.name == '') {
                this.filtered = this.partners
                this.dialog = true
            }else {
                this.filtered = this.partners.filter((i) => i.name.toLowerCase().includes(this.partner.name.toLowerCase()))
                if (this.filtered.length == 1) {
                    this.setItem(this.filtered[0].id)
                }else {
                    this.dialog = true
                }
            }
        },
        setItem (id) {
            let item = this.partners.filter((i) => {
                return i.id == id
            })[0]
            this.partner.id = item.id
            this.partner.name = item.name
            this.dialog = false
        }    
    }
}
</script>
