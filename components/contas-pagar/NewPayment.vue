<template>
    <v-dialog max-width="75vw">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" >
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Add Conta a Pagar
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card>
            <v-card-title>
                Nova Conta a Pagar
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validationc>
                    <v-row dense>
                        <v-col cols="2">
                            <DateField ref="dateEmit" :mainLabel="'Data Emissão'" @changeEmit="validDate" />
                        </v-col>
                        <v-col cols="2">
                            <DateField ref="dateDigi" :mainLabel="'Data Digitação'" @changeEmit="validDate" />
                        </v-col>
                        <Partner ref="partner" cols="5" />
                        <Expense ref="expense" cols="3"/>
                        <PaymentForm :cols="3" />
                        <PaymentCondition :cols="3" />
                        <v-col cols="2">
                            <v-text-field filled dense label="Valor" type="number"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <DateField ref="dateParc" :mainLabel="'1ª parcela em'" :prepend-icon="''" />
                        </v-col>
                        <v-col cols="2" class="d-flex justify-space-around">
                            <v-btn
                            color="primary"
                            fab
                            @click="setForm">
                                <v-icon>mdi-plus</v-icon>
                            </v-btn>
                            <v-btn
                            color="success"
                            fab>
                                <v-icon>mdi-check</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
                <v-data-table dense 
                :headers="[
                    { text: 'Parcelas', align: 'center', justify: 'center', value: 'parcelas' },
                    { text: 'Nome', value: 'name' },
                    { text: 'Ações', value: 'actions' },
                ]" 
                hide-default-footer fixed-header :items-per-page="-1"></v-data-table>
            </v-card-text>
        </v-card>
        <v-alert v-if="messageError" dismissible color="red">{{ messageError }}</v-alert>
    </v-dialog>
</template>

<script>
import DateField from '../default/DateField.vue';
import SearchText from '../default/SearchText.vue';
import PaymentCondition from './actions/PaymentCondition.vue';
import PaymentForm from './actions/PaymentForm.vue';
import Expense from './actions/Expense.vue';
import Partner from './actions/Partner.vue';
export default {
    name: 'NewPayment',
    data: () => ({
        valid: false,
        activePicker: null,
        date: null,
        menu: false,

        messageError: null,
        form: {},
    }),
    components: {
        SearchText,
        DateField,
        PaymentCondition,
        PaymentForm,
        Expense,
        Partner
    },
    methods: {
        setForm () {
            this.form = {
                date_digi: this.$refs.dateDigi.date,
                date_emit: this.$refs.dateEmit.date,
                partner: this.$refs.partner.$refs.partner.item.id,
                expense: this.$refs.expense.$refs.expense.item.id
            }
            console.log(this.form)
            // console.log(this.$refs.partner.$refs.partner.item.id)
        },
        validDate () {
            if (this.$refs.dateDigi.date < this.$refs.dateEmit.date) {
                this.messageError = 'Data de emissão não pode ser maior que a data de digitação!'
                this.$refs.dateDigi.date = null
                this.$refs.dateEmit.date = null
            }
        }
    }
}
</script>

<style>

</style>