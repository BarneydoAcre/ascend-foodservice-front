<template>
    <div class="main-cadastros">
        <Actions @getProduct="$refs.product.getProduct()" @getProductSale="$refs.productsale.getProductsSale()"></Actions>
        <Product ref="product"></Product>
        <ProductSale ref="productsale"></ProductSale>
    </div>
</template>

<script>
import Actions from "../../components/cadastros/Actions.vue";
import Product from "../../components/cadastros/Product.vue";
import ProductSale from "../../components/cadastros/ProductSale.vue";
export default {
    name: "cadastros",
    components: {
        Product,
        Actions,
        ProductSale,
    },
}
</script>

<style>
.main-cadastros {
    display: grid;
    grid-template-columns: 5% 23.75% 23.75% 23.75% 23.75%;
    grid-template-rows: 24vh 24vh 24vh 24vh;
    grid-template-areas: 'actions product product productsale productsale '
                         'actions product product productsale productsale'
                         'actions product product productsale productsale'
                         'actions product product productsale productsale';
}
</style>