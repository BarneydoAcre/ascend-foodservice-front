<template>
    <div>
        <v-card-title>Criar um novo Produto</v-card-title>
        <v-card-text>
            <v-form v-model="valid" ref="form" lazy-validation>
                <v-row dense>
                    <v-col cols="8">
                        <v-text-field dense filled label="Nome do Produto" v-model="form.name" :rules="rules"></v-text-field>
                    </v-col>
                    <v-col cols="3">
                        <v-select dense filled label="Marca" @focus="getBrand" v-model="form.brand" :rules="rules" :items="brands" item-text="brand_name" item-value="brand_id"></v-select>
                    </v-col>
                    <v-col cols="1">
                        <AddBrand @getBrand="getBrand"></AddBrand>
                    </v-col>
                    <v-col cols="3">
                        <v-select dense filled label="Und. Medida" @focus="getMeasure" v-model="form.measure" :rules="rules" :items="measures" item-text="measure_name" item-value="measure_id"></v-select>
                    </v-col>
                    <v-col cols="1">
                        <AddMeasure @getMeasure="getMeasure"></AddMeasure>
                    </v-col>
                    <v-col cols="3">
                        <v-text-field dense filled label="Qtd. Atual" v-model="form.stock" :rules="rules"></v-text-field>
                    </v-col>
                    <v-col cols="23">
                        <v-text-field dense filled label="Custo" v-model="form.cost" :rules="rules"></v-text-field>
                    </v-col>
                    <v-col cols="1" class="d-flex justify-center">
                        <v-btn fab color="success" @click="addProduct">
                            <v-icon>mdi-check</v-icon>
                        </v-btn>
                    </v-col>
                    <v-col cols="1" class="d-flex justify-center">
                        <v-btn fab color="warning" @click="reset">
                            <v-icon>mdi-close</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-form>
        </v-card-text>
    </div>
</template>

<script>
import AddBrand from './AddBrand.vue'
import AddMeasure from './AddMeasure.vue';
export default {
    name: "AddProduct",
    components: {
    AddBrand,
    AddMeasure
},
    emits: ["getProduct"],
    data() {
        return {
            dialog: false,
            valid: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                type: 1,
                name: null,
                brand: null,
                measure: null,
                stock: null,
                cost: null,
            },
            rules: [
                v => !!v || "Obrigatório",
            ],
            brands: [],
            measures: []
        };
    },
    mounted() {
    },
    methods: {
        reset() {
            this.$refs.form.reset();
        },
        validate() {
            this.$refs.form.validate();
        },
        reset () {
            this.$refs.form.reset();
        },
        addProduct() {
            this.validate();
            setTimeout(async () => {
                console.log(this.form)
                if (this.valid != false) {
                    const req = await fetch(process.env.HOST_BACK + "/register/addProduct/", {
                        method: "POST",
                        body: JSON.stringify(this.form),
                        headers: { "Content-Type": "application/json" },
                    });
                    if (req.status == 200) {
                        this.dialog = false;
                        this.$emit("getProduct");
                        this.reset()
                    }
                    else {
                    }
                }
            }, 0);
        },
        async getBrand() {
            const req = await fetch(process.env.HOST_BACK + "/register/getBrand/?" + new URLSearchParams({
                token: localStorage.getItem("refresh"),
                company: localStorage.getItem("company"),
            }), {
                method: "GET",
            });
            const res = await req.json();
            this.brands = res;
        },
        async getMeasure() {
            const req = await fetch(process.env.HOST_BACK + "/register/getMeasure/?" + new URLSearchParams({
                token: localStorage.getItem("refresh"),
                company: localStorage.getItem("company"),
            }), {
                method: "GET",
            });
            const res = await req.json();
            this.measures = res;
        }
    },
}
</script>
