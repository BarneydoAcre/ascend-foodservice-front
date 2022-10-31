<template>
    <v-dialog v-model="dialog" max-width="94vw" class="main-cadastros-customer">
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
                    Campanhas
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card>
            <v-card-text>
                <v-form>
                    <v-row dense>
                        <v-col cols="12">
                            <v-text-field label="Nome"></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-checkbox label="Ativa"></v-checkbox>
                        </v-col>
                        <v-col cols="12">
                            <v-select label="Tipo"></v-select>
                        </v-col>
                        <v-col cols="12" lg="6">
                            <v-menu
                            ref="from_at"
                            v-model="from_at_menu"
                            :close-on-content-click="false"
                            transition="scale-transition"
                            offset-y
                            max-width="290px"
                            min-width="auto">
                            <template v-slot:activator="{ on, attrs }">
                                <v-text-field
                                v-model="dateFormatted"
                                label="Date"
                                hint="MM/DD/YYYY format"
                                persistent-hint
                                prepend-icon="mdi-calendar"
                                v-bind="attrs"
                                v-on="on"></v-text-field>
                            </template>
                            <v-date-picker
                                v-model="date"
                                no-title
                                @input="from_at_menu = false"></v-date-picker>
                            </v-menu>
                            <p>Date in ISO format: <strong>{{ date }}</strong></p>
                        </v-col>
                    </v-row>
                </v-form>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'AddCampaign',
    emits: ["getPartner"],
    components: {
    },
    data () {
        return {
            from_at_menu: false,
            dialog: false,
            filtered: [],
            loadingSelect: false,
            date: null,
            dateFormatted: null,
            ruleName: [
                v => !!v || "Obrigatório",
                v => (v && v.length <= 50) || "Limite de caracteres",
            ],
            ruleData: [
                v => !!v || "Obrigatório",
                v => (v && v.length <= 20) || "Limite de caracteres",
            ],
            ruleNum: [
                v => !!v || "Obrigatório",
                v => (v && v.length <= 5) || "Limite de caracteres",
            ],
            ruleCep: [
                v => !!v || "Obrigatório",
                v => (v && v.length <= 8) || "Limite de caracteres",
                ],
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
                { text: "Cidade", value: "name" },
                { text: "Ações", value: "actions" },
            ],
        }
    },
    methods: {
        reset () {
            this.$refs.form.reset()
        },
        validate () {
            this.$refs.form.validate()
        },
        cancelButton () {
            // this.reset()
            this.dialog = false
            this.e1 = 1
        },
        nextButton () {
            // this.validate()
            setTimeout(() => {
                if (this.validForm == true) {
                    this.e1 += 1
                }
            }, 0)
        },
        async addPartner () {
            this.form.city = this.form.city.id
            const req = await fetch(process.env.HOST_BACK+'/register/addPartner/',{
                method: "POST",
                body: JSON.stringify(this.form),
                headers: { "Content-Type": "application/json" },
            })
            
            if (req.status == 200) {
                this.dialog = false
                this.e1 += 1
                this.form.city = { id: null, name: null }
                this.$emit("getPartner")
            }
        },
        async getCities() {
            this.loadingSelect = true
            const req = await fetch(process.env.HOST_BACK+'/default/getCities/?'+new URLSearchParams({
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
            if(this.form.city.name == null || this.form.city.name == '') {
                this.filtered = this.cities
                this.dialogCity = true
            }else {
                this.filtered = this.cities.filter((i) => i.name.toLowerCase().includes(this.form.city.name.toLowerCase()))
                this.filtered = this.cities
                if (this.filtered.length == 1) {
                    this.setItem(this.filtered[0].id)
                    this.dialogCity = false
                }else {
                    this.dialogCity = true
                }
            }
        },
        setItem (id) {
            let item = this.cities.filter((i) => {
                return i.id == id
            })[0]
            this.form.city.id = item.id
            this.form.city.name = item.name
            this.dialogCity = false
        } 
    }
}
</script>