<template>
    <div class="main-cadastros">
        <v-card width="25vw" height="95vh">
            <v-card-title>Produtos</v-card-title>
            <v-card-text>
                <v-list dense nav>
                    <Product></Product>
                    <ProductSale></ProductSale>
                    <Customer></Customer>
                </v-list>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
import Product from "../../components/cadastros/Product.vue";
import ProductSale from "../../components/cadastros/ProductSale.vue";
import Customer from "../../components/cadastros/Customer.vue";
export default {
    name: "cadastros",
    components: {
        Product,
        ProductSale,
        Customer
    },
}
</script>

<style>
.main-cadastros {
    display: grid;
    grid-template-columns: 25% 25% 25% 25%;
    grid-template-rows: 24vh 24vh 24vh 24vh;
    grid-template-areas: 'product product productsale productsale '
                         'product product productsale productsale'
                         'product product productsale productsale'
                         'product product productsale productsale';
}
</style>