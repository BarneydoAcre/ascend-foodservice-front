<template>
    <v-menu
        ref="menu"
        v-model="menu"
        :close-on-content-click="false"
        transition="scale-transition"
        offset-y
        min-width="auto">
        <template v-slot:activator="{ on, attrs }">
            <v-text-field
                filled
                dense
                v-model="date"
                label="Data Competência"
                prepend-icon="mdi-calendar"
                readonly
                v-bind="attrs"
                v-on="on"
            ></v-text-field>
        </template>
        <v-date-picker
            v-model="date"
            :active-picker.sync="activePicker"
            :max="(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)"
            min="1950-01-01"
            @change="save"></v-date-picker>
    </v-menu>
</template>

<script>
export default {
    name: 'DateField',
    data: () => ({
        activePicker: null,
        date: null,
        menu: false,
    }),
    watch: {
        menu (val) {
            val && setTimeout(() => (this.activePicker = 'YEAR'))
        },
    },
    methods: {
        save (date) {
            this.$refs.menu.save(date)
        },
    },

}
</script>

<style>

</style>