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
                    Clientes
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-stepper v-model="e1">
            <v-stepper-header>
                <v-stepper-step :complete="e1 > 1" step="1">
                    Dados do cliente
                </v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step step="2">
                    Configurações do cliente
                </v-stepper-step>
            </v-stepper-header>
            <v-stepper-items>
                <v-stepper-content step="1">
                    <v-card class="mb-12" height="58vh">
                        <v-card-text>
                            <v-form v-model="validForm" ref="form" lazy-validation>
                                <v-row dense v-if="form.person_f_j">
                                    <v-col cols="6">
                                        <v-switch v-model="form.person_f_j" label="Física/Jurídica"></v-switch>
                                    </v-col>
                                    <v-col cols="6" class="d-flex justify-space-around">
                                        <v-checkbox v-model="form.type_client" label="Cliente"></v-checkbox>
                                        <v-checkbox v-model="form.type_provider" label="Fornecedor"></v-checkbox>
                                        <v-checkbox v-model="form.type_conveyor" label="Transportador"></v-checkbox>
                                    </v-col>
                                    <v-col cols="6">
                                        <v-text-field filled dense counter="50" :rules="ruleName" v-model="form.name" label="Razão Social"></v-text-field>
                                    </v-col>
                                    <v-col cols="6">
                                        <v-text-field filled dense counter="50" :rules="ruleName" v-model="form.fantasy" label="Fantasia"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled dense counter="20" :rules="ruleData" v-model="form.cnpj" label="CNPJ"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled dense counter="20" :rules="ruleData" v-model="form.ie" label="IE"></v-text-field>
                                    </v-col>
                                    <v-col cols="6">
                                        <v-text-field filled dense counter="50" v-model="form.email" label="Email"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled dense counter="20" :rules="ruleData" v-model="form.phone_number" label="Contato"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled dense counter="8" :rules="ruleCep" v-model="form.cep" label="CEP"></v-text-field>
                                    </v-col>
                                    <v-col cols="4">
                                        <v-text-field filled dense counter="50" :rules="ruleName" v-model="form.street" label="Rua"></v-text-field>
                                    </v-col>
                                    <v-col cols="1">
                                        <v-text-field filled dense counter="5" :rules="ruleNum" v-model="form.num" label="Nº"></v-text-field>
                                    </v-col>
                                    <v-col cols="3">
                                        <v-text-field filled dense counter="50" :rules="ruleName" v-model="form.district" label="Bairro"></v-text-field>
                                    </v-col>
                                </v-row>
                                <v-row dense v-else>
                                    <v-col cols="6">
                                        <v-switch v-model="form.person_f_j" label="Física/Jurídica"></v-switch>
                                    </v-col>
                                    <v-col cols="6" class="d-flex justify-space-around">
                                        <v-checkbox v-model="form.type_client" label="Cliente"></v-checkbox>
                                        <v-checkbox v-model="form.type_provider" label="Fornecedor"></v-checkbox>
                                        <v-checkbox v-model="form.type_conveyor" label="Transportador"></v-checkbox>
                                    </v-col>
                                    <v-col cols="4">
                                        <v-text-field filled dense counter="50" :rules="ruleName" v-model="form.name" label="Nome"></v-text-field>
                                    </v-col>
                                    <v-col cols="4">
                                        <v-text-field filled dense counter="50" :rules="ruleName" v-model="form.fantasy" label="Apelido"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled counter="20" dense v-model="form.cpf" label="CPF"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled counter="20" dense v-model="form.rg" label="RG"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field filled dense counter="20" v-model="form.phone_number" label="Contato"></v-text-field>
                                    </v-col>
                                    <v-col cols="4">
                                        <v-text-field filled dense counter="50" v-model="form.email" label="Email"></v-text-field>
                                    </v-col>
                                    <v-col cols="2">
                                        <v-text-field append-icon="mdi-plus" filled dense counter="8" v-model="form.cep" label="CEP"></v-text-field>
                                    </v-col>
                                    <v-col cols="4">
                                        <v-text-field filled dense counter="50" label="Rua"></v-text-field>
                                    </v-col>
                                    <v-col cols="1">
                                        <v-text-field filled dense counter="5" label="Nº"></v-text-field>
                                    </v-col>
                                    <v-col cols="3">
                                        <v-text-field filled dense counter="50" label="Bairro"></v-text-field>
                                    </v-col>
                                    <v-col cols="3"
                                    color="primary">
                                        <v-text-field dense filled :loading="loadingSelect" @click="getCities" @keydown.enter="filterCity" label="Cidade" v-model="form.city.name"></v-text-field>
                                    </v-col>
                                    <v-dialog v-model="dialogCity" max-width="45vw">
                                        <v-card>
                                            <v-card-title>Cidades</v-card-title>
                                            <v-card-text>
                                                <v-form v-model="valid" ref="form" lazy-validation>
                                                    <v-row dense>
                                                        <v-col cols="12">
                                                            <v-text-field dense filled label="Digite o nome da cidade" v-model="form.city.name" @keyup="filterCity"></v-text-field>
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
                                </v-row>
                            </v-form>
                        </v-card-text>
                    </v-card>
                    <div class="d-flex justify-end">
                        <div class="d-flex justify-space-around">
                            <v-btn text @click="dialog = false">
                                Cancelar
                            </v-btn>
                            <v-btn v-if="validForm" color="green" class="ml-4" @click="nextButton">
                                Próximo
                            </v-btn>
                            <v-btn v-else disabled color="green" class="ml-4" @click="1">
                                Validar
                            </v-btn>
                        </div>
                    </div>
                </v-stepper-content>

                <v-stepper-content step="2">
                    <v-card class="mb-12" height="58vh">
                        <v-card-text>
                            <p>Cadastro Ok!</p>
                        </v-card-text>
                    </v-card>
                    <div class="d-flex justify-end">
                        <div class="d-flex justify-space-around">
                            <v-btn text @click="e1 -= 1">
                                Voltar
                            </v-btn>
                            <v-btn v-if="validForm" color="primary" class="ml-4" @click="addPartner">
                                Gravar Dados
                            </v-btn>
                        </div>
                    </div>
                </v-stepper-content>
            </v-stepper-items>
        </v-stepper>
    </v-dialog>
</template>

<script>
export default {
    name: 'AddCustomer',
    emits: ["getPartner"],
    components: {
    },
    data () {
        return {
            dialog: false,
            dialogCity: false,
            valid: false,
            validForm: false,
            filtered: [],
            loadingSelect: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                token: localStorage.getItem('refresh'),
                person_f_j: false,
                type_client: false,
                type_provider: false,
                type_conveyor: false,
                name: null,
                fantasy: null,
                cpf: null,
                rg: null,
                cnpj: null,
                ie: null,
                phone_number: null,
                email: null,
                cep: null,
                street: null,
                district: null,
                city: { id: null, name: null },
                num: null,
            },
            e1: 1,
            steps: 2,
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