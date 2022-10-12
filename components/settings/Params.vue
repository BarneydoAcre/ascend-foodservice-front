<template>
    <div class="main-settings-params">
        <v-card width="25vw">
            <v-card-title>Módulo Suporte</v-card-title>
            <v-card-text>
                <v-list dense nav>
                    <!-- aqui irá os botões -->
                </v-list>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
export default {
    name: "Params",
}
</script>

<style scoped>
.main-settings-params {
    grid-area: params;
}
.card-suport {
    border: 1px solid #d8d8d8;
    border-radius: 1vh;
    padding: 1vh;
    width: 40%;
}
</style>