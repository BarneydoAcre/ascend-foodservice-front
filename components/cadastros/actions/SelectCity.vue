<template>
    <v-dialog v-model="dialog" max-width="45vw">
        <template v-slot:activator="{ on, attrs }">
            <v-col cols="3"
            color="primary" 
            v-bind="attrs"
            v-on="on">
                <v-text-field dense filled :loading="loadingSelect" @click="getCities" @keydown.enter="filterCity" label="Prato" v-model="city.name" :rules="rules"></v-text-field>
            </v-col>
        </template>
        <v-card>
            <v-card-title>Componentes do Produto</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="12">
                            <h3>Prato</h3>
                        </v-col>
                        <v-col cols="12">
                            <v-text-field dense filled label="Produto" v-model="city.name" @keyup="filterCity"></v-text-field>
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
    name: "SelectCity",
    props: ["city"],
    data () {
        return {
            loadingSelect: false,
            valid: false,
            cities: [],
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
                { text: "Produto", value: "name" },
                { text: "Preço", value: "cep" },
                { text: "Ações", value: "actions" },
            ],
        }
    },
    mounted () {
        this.getCities()
    },
    methods: {
        reset () {
            this.$refs.form.reset();
        },
        async getCities() {
            this.loadingSelect = true
            const req = await fetch(process.env.HOST_BACK+'/register/getCities/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.cities = res
            this.loadingSelect = false
        },
        filterCity () {
            if(this.city.name == null || this.city.name == '') {
                this.filtered = this.cities
                this.dialog = true
            }else {
                this.filtered = this.cities.filter((i) => i.name.toLowerCase().includes(this.city.name.toLowerCase()))
                if (this.filtered.length == 1) {
                    this.setItem(this.filtered[0].id)
                }else {
                    this.dialog = true
                }
            }
        },
        setItem (id) {
            let item = this.cities.filter((i) => {
                return i.id == id
            })[0]
            this.city.id = item.id
            this.city.name = item.name
            this.dialog = false
        }    
    }
}
</script>
