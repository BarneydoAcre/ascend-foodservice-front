<template>
    <v-card-text class="list-products">
        <v-card-title>Produtos para Venda</v-card-title>
        <v-data-table fixed-header height="76vh" :loading="loadingTableList" :headers="headers" :items="prodSale" :items-per-page="-1" hide-default-footer>
            <template v-slot:item.actions="{ item }">
                <v-icon 
                small 
                @click="$emit('editProduct', item.id)">mdi-pencil</v-icon>
                <v-icon 
                small 
                @click="$emit('deleteProduct', item.id)">mdi-delete</v-icon>
            </template>
        </v-data-table>
    </v-card-text>
</template>

<script>
export default {
    name: "ListProductSale",
    props: ["prodSale", "loadingTableList"],
    emits: ["editProduct"],
    data () {
        return {
            loadingTable: false,
            headers: [
                { text: "ID", align: "center", justify: "center", value: "id" },
                { text: "Produto", align: "center", justify: "center", value: "name" },
                { text: "Custo", align: "center", justify: "center", value: "cost" },
                { text: "Preço", align: "center", justify: "center", value: "price" },
                { text: "Ações", align: "center", justify: "center", value: "actions" },
            ],
        }
    },
}
</script>

<style>

</style>