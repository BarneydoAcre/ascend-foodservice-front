<template>
    <div class="Sale-Filter">
        <v-form v-model="valid" ref="form" lazy-validation>
            <v-row dense>
                <v-col cols="4">
                    <v-menu ref="menu"
                    v-model="menu"
                    :close-on-content-click="false"
                    transition="scale-transition"
                    offset-y
                    min-width="auto">
                        <template v-slot:activator="{ on, attrs }">
                            <v-text-field
                            dense
                            filled
                            readonly
                            label="Período"
                            v-model="form.date"
                            v-bind="attrs"
                            v-on="on"
                            ></v-text-field>
                        </template>
                        <v-date-picker
                            v-model="form.date"
                            :active-picker.sync="activePicker"
                            :max="(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)"
                            min="1950-01-01"
                            range
                            @change="save"
                        ></v-date-picker>
                    </v-menu>
                </v-col>
                <v-col cols="3">
                    <v-text-field
                    dense
                    filled
                    label="Nº da Venda"
                    v-model="form.sale"></v-text-field>
                </v-col>
                <v-col cols="5" class="d-flex justify-center">
                    <v-btn 
                    fab
                    color="primary"
                    @click="applyFilter(form)">
                        <v-icon>mdi-filter</v-icon></v-btn>
                    <v-btn 
                    fab
                    color="warning"
                    class="ml-4"
                    @click="clearFilter">
                        <v-icon>mdi-broom</v-icon>
                    </v-btn>
                </v-col>
            </v-row>
        </v-form>
    </div>
    
</template>

<script>
export default {
    name: 'FilterSales',
    props: [],
    emits: ["filterSales",],
    data () {
        return {
            dialog: false,
            valid: false,
            menu: false,
            activePicker: null,
            form: {
                date: null,
                sale: null,
            },
        }
    },
    methods: {
        save (date) {
            this.$refs.menu.save(date)
        },
        applyFilter (form) {
            history.pushState({}, null, '?'+new URLSearchParams({
                date: form.date,
                sale: form.sale,
            }))
            this.dialog = false
            this.$emit('filterSales')
        },
        clearFilter () {
            history.pushState({}, null, '?'+new URLSearchParams({
                date: 'null',
                sale: 'null'
            }))
            this.dialog = false
            this.$emit('filterSales')

        },

    },

}
</script>

<style>
.Sale-Filter {
    grid-area: filter;
}
</style>