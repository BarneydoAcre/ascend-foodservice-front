<template>
    <v-dialog v-model="dialog" max-width="45vw">
        <template v-slot:activator="{ on, attrs }">
            <div 
            color="primary" 
            v-bind="attrs">
                <v-text-field dense filled :loading="loading" @keydown="$emit('getPartner')" @keydown.enter="filter" :label="subLabel" v-model="item.name" :rules="rules"></v-text-field>
            </div>
        </template>
        <v-card>
            <v-card-title>{{ mainLabel }}</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="12">
                            <v-text-field dense filled label="Digite aqui" v-model="item.name" @keyup="filter"></v-text-field>
                        </v-col>
                    </v-row>
                </v-form>
                <div style="max-height: 45vh; overflow-y: auto;">
                    <v-data-table
                    dense 
                    :loading="loading"
                    class="elevation-1" 
                    :headers="headers" 
                    :items-per-page="-1" 
                    :items="filtered" 
                    hide-default-footer>
                        <template v-slot:item.actions="{ item }">
                            <v-icon small @click="setItem(item.id)">mdi-plus</v-icon>
                        </template>
                    </v-data-table>
                </div>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'SearchText',
    emits: ['getPartner'],
    props: {
        loading: {
            type: Boolean,
            default: () => { return true }
        },
        items: {
            type: Array,
            default: () => { return [] }
        },
        headers: {
            type: Array,
            default: () => { return [] }
        },
        mainLabel: {
            type: String,
            default: 'No data'
        },
        subLabel: {
            type: String,
            default: 'No data'
        },
    },
    data () {
        return {
            valid: false,
            dialog: false,
            item: { id: null, name: null },
            quantity: null,
            filtered: [
                {
                    id: 1,
                    name: 'teste'
                }
            ],
            rules: [
                v => !!v || "Obrigatório",
            ],
        }
    },
    methods: {
        filter () {
            if(this.item.name == null || this.item.name == '') {
                this.filtered = this.items
                this.dialog = true
            }else {
                this.filtered = this.items.filter((i) => i.name.toLowerCase().includes(this.item.name.toLowerCase()))
                if (this.filtered.length == 1) {
                    this.setItem(this.filtered[0].id)
                }else {
                    this.dialog = true
                }
            }
        },
        setItem (id) {
            let it = this.items.filter((i) => {
                return i.id == id
            })[0]
            this.item.id = it.id
            this.item.name = it.name
            this.dialog = false
        }
    }
}
</script>

<style>

</style>