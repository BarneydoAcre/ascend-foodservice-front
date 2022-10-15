<template>
    <div class="TableSales">
        <v-card>
            <v-card-title>Vendas</v-card-title>
            <div class="ListSales">
                <v-card-text>
                    <v-expansion-panels accordion>
                        <v-expansion-panel
                        v-for="(item,i) in sales"
                        :key="i">
                            <v-expansion-panel-header class="d-flex justify-space-around">
                                <div>Venda: {{ item.id }}</div>  
                                <div>Valor: {{ item.value}}</div>  
                                <div>Entrega: {{ item.delivery }}</div> 
                                <div>Total: {{ item.value+item.delivery }}</div>  
                                <div class="d-flex justify-center">
                                    <v-btn
                                        fab
                                        small
                                        color="primary"
                                        :href="host+'/sale/print/'+item.id"
                                        target="_blank">
                                        <v-icon>mdi-printer</v-icon>
                                    </v-btn> 
                                </div>
                            </v-expansion-panel-header>
                            <v-expansion-panel-content
                            v-for="(salei,i) in saleItems.filter((i) => { return i.sale == item.id })"
                            :key="i">
                                <v-card>
                                    <v-card-text class="d-flex flex-row justify-space-between">
                                        <div>Produto: {{ salei.product_id }}-{{ salei.product_name }}</div>  
                                        <div>Valor: {{ salei.price }}</div>  
                                        <div>Qtd.: {{ salei.quantity }}</div>   
                                        <div>Total: {{ salei.price*salei.quantity }}</div>   
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
export default {
    name: 'ListSales',
    props: [
        "sales",
        "saleItems",
    ],
    data () {
        return {
            loadingTableList: false,
            host: process.env.HOST_BACK,
            headers: [
                { text: "ID", align: "center", justify: "center", value: "id" },
                { text: "Total", align: "center", justify: "center", value: "total" },
                { text: "Ações", align: "center", justify: "center", value: "actions" },
            ],

        }
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