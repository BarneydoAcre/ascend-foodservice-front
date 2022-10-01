<template>
    <v-dialog v-model="dialog" max-width="55vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            v-bind="attrs" 
            v-on="on"
            height="70%">
                <v-icon>mdi-plus</v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                Nova Marca
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="10">
                            <v-text-field dense filled label="Nome da Marca" v-model="form.brand" :rules="rules"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn 
                            fab
                            color="green"
                            height="70%"
                            @click="addBrand">
                                <v-icon>mdi-send</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: "Brand",
    emits: ["getBrand",],
    data () {
        return {
            dialog: false,
            valid: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                brand: null,
            },
            rules: [
                v => !!v || "Obrigatório",
            ],
        }
    },
    methods: {
        validate () {
            this.$refs.form.validate()
        },
        addBrand () {
            this.validate();
            setTimeout(async () => {
                if (this.valid != false) {
                    const req = await fetch(process.env.HOST_BACK + "/register/addBrand/", {
                        method: "POST",
                        body: JSON.stringify(this.form),
                        headers: { "Content-Type": "application/json" },
                    });
                    if (req.status == 200) {
                        this.dialog = false;
                        this.$emit("getBrand");
                    }
                    else {
                    }
                }
            }, 0);
        },
    }
    
}
</script>