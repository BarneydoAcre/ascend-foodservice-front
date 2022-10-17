<template>
    <v-dialog v-model="dialog" max-width="40vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn
                fab
                small
                color="error"
                v-bind="attrs" 
                v-on="on">
                <v-icon>mdi-delete</v-icon>
            </v-btn> 
        </template>
            <v-card>
                <v-card-title>CUIDADO!</v-card-title>
                <v-card-text>Quer mesmo cancelar essa venda?</v-card-text>
                <v-card-actions class="d-flex justify-space-around">
                    <v-btn
                        color="primary"
                        @click="dialog = false">Manter Venda
                    </v-btn> 
                    <v-btn
                        color="error"
                        @click="deleteSale(sale)">
                        <v-icon>mdi-delete</v-icon> Cancelar
                    </v-btn> 
                </v-card-actions>
            </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: "DeleteSaleNotification",
    props: [
        "sale",
    ],
    data () {
        return {
            dialog: false,
        }
    },
    methods: {
        async deleteSale(id) {
            let data = JSON.stringify({
                company: localStorage.getItem("company"),
                token: localStorage.getItem("refresh"),
                sale: id,
            });
            const req = await fetch(process.env.HOST_BACK + "/sale/deleteSale/", {
                method: "POST",
                body: data,
                headers: { "Content-Type": "application/json" },
            });
            if (req.status == 200) {
                this.dialog = false;
                this.$emit('getSale')
            }
        }
    },
}
</script>