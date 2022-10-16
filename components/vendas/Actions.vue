<template>
    <div class="Sales-Actions">
        <v-card height="12vh" class="d-flex align-center">
            <v-btn
            fab
            small
            color="primary"
            class="ml-2">
                <v-icon>mdi-alphabetical</v-icon>
            </v-btn>
            <v-btn
            fab
            small
            color="primary"
            class="ml-2">
                <v-icon>mdi-microsoft-excel</v-icon>
            </v-btn>

        </v-card>
    </div>
</template>

<script>
export default {
    name: "Actions",
}
</script>

<style>
.Sales-Actions {
    grid-area: actions;
}
</style>