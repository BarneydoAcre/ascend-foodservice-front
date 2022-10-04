<template>
    <div class="main-sale">
    </div>
</template>

<script>
export default {
    name: "contas-receber",
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
