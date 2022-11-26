<template>
    <div class="content">
        <v-card height="100%" class="d-flex flex-column justify-space-around align-center">
            <v-card-text class="d-flex flex-column justify-center align-center">
                <p class="content-title">Bem Vindo a CloudSys!</p>
                <p class="content-text"></p>
            </v-card-text>
            <v-btn :to="'/team/'"></v-btn>
        </v-card>
    </div>
</template>

<script>
export default {
    name: 'ContentIndex',

}
</script>

<style scoped>
.content {
    grid-area: content;
    
    padding: 8vh 4vw 8vh 6vw;
}
</style>