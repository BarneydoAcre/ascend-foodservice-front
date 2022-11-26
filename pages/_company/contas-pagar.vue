<template>
    <div class="main-contas-pagar">
        <v-card width="25vw" height="95vh">
            <v-card-title>Ações</v-card-title>
            <v-card-text>
                <v-list dense nav>
                    <NewPayment :disabled="false"></NewPayment>
                    
                </v-list>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
import NewPayment from '../../components/contas-pagar/NewPayment.vue';
export default {  
    name: 'ContasPagar',
    components: {
        NewPayment,
    }  

}
</script>

<style>
.main-contas-pagar {
    display: grid;
    grid-template-columns: 25% 25% 25% 25%;
    grid-template-rows: 24vh 24vh 24vh 24vh;
    grid-template-areas: 'actions product productsale productsale '
                         'actions product productsale productsale'
                         'actions product productsale productsale'
                         'actions product productsale productsale';
}
</style>