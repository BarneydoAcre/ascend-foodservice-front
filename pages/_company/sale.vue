<template>
    <div class="main-sale">
        <Actions @getProduct="$refs.product.getProduct()" @getProductSale="$refs.productsale.getProductsSale()"></Actions>
        <Product ref="product"></Product>
        <ProductSale ref="productsale"></ProductSale>
        <Ticket ref="sale" @getProduct="$refs.product.getProduct()"></Ticket>
    </div>
</template>

<script>
import Actions from "../../components/sale/Actions.vue";
import Product from "../../components/sale/Product.vue";
import ProductSale from "../../components/sale/ProductSale.vue";
import Ticket from "../../components/sale/Ticket.vue";
export default {
    name: "Sale",
    components: {
        Product,
        Actions,
        ProductSale,
        Ticket
    },
}
</script>

<style>
.main-sale {
    display: grid;
    grid-template-columns: 25% 25% 20% 20% 10%;
    grid-template-rows: 9vh 14.5vh 29vh 14.5vh 29vh;
    grid-template-areas: 'actions actions graphs graphs graphs'
                         'product product none none none'
                         'product product sale sale sale'
                         'productsale productsale sale sale sale'
                         'productsale productsale sale sale sale';
}
</style>
