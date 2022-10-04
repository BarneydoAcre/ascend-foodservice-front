<template>
    <div class="main-caixa">
        <Ticket @getProduct="$refs.product.getProduct()"></Ticket>
    </div>
</template>

<script>
import Ticket from "../../components/caixa/Ticket.vue";
export default {
    name: "caixa",
    components: {
        Ticket
    }
}
</script>

<style>
.main-caixa {
    display: grid;
    grid-template-columns: 100%;
    grid-template-rows: 95vh;
    grid-template-areas: 'caixa';
}
</style>
