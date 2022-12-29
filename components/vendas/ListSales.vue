<template>
    <div class="TableSales">
        <v-card>
            <v-card-title>Vendas</v-card-title>
            <div class="ListSales">
                <v-card-text>
                    <v-expansion-panels accordion>
                        <v-expansion-panel
                        v-for="(s,i) in sales"
                        :key="i">
                            <v-expansion-panel-header class="d-flex justify-space-around">
                                <div>Venda: {{ s.sale }}</div>  
                                <div>Valor: {{ s.value}}</div>  
                                <div>Entrega: {{ s.delivery }}</div> 
                                <div>Total: {{ s.value + s.delivery }}</div>  
                                <div>Data: {{ s.created.split(' ')[0] }}</div>  
                                <div class="d-flex justify-space-around">
                                    <v-btn
                                        fab
                                        small
                                        color="primary"
                                        :href="host+'/print/sale/'+s.sale"
                                        target="_blank">
                                        <v-icon>mdi-printer</v-icon>
                                    </v-btn> 
                                    <v-btn
                                        fab
                                        small
                                        color="warning">
                                        <v-icon>mdi-pencil</v-icon>
                                    </v-btn> 
                                    <DeleteSaleNotification 
                                    @getSale="$emit('getSale')"
                                    :sale="s.sale"></DeleteSaleNotification>
                                </div>
                            </v-expansion-panel-header>
                            <v-expansion-panel-content v-for="(si,i) in s.products"
                            :key="i">
                                <v-card>
                                    <v-card-text class="d-flex flex-row justify-space-between">
                                        <div>Produto: {{ si.id }}-{{ si.name }}</div>  
                                        <div>Valor: {{ si.price }}</div>  
                                        <div>Qtd.: {{ si.quantity }}</div>   
                                        <div>Total: {{ si.price*si.quantity }}</div>   
                                    </v-card-text>
                                </v-card>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </v-card-text>
            </div>
        </v-card>
    </div>
</template>

<script>
import DeleteSaleNotification from './actions/DeleteSaleNotification.vue'
export default {
    name: "ListSales",
    props: [
        "sales",
    ],
    emits: [
        "getSale",
    ],
    components: { 
        DeleteSaleNotification ,
    },
    data() {
        return {
            dialogDelSale: false,
            loadingTableList: false,
            host: process.env.HOST_BACK,
            headers: [
                { text: "ID", align: "center", justify: "center", value: "id" },
                { text: "Total", align: "center", justify: "center", value: "total" },
                { text: "Ações", align: "center", justify: "center", value: "actions" },
            ],
        };
    },
}
</script>

<style>
.TableSales {
    grid-area: table;
}
.ListSales {
    overflow-y: auto;
    max-height: 55vh;
}
</style>