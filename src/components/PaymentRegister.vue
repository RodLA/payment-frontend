<template>
    <div>
        <v-row>
            <v-col cols="12" md="12">
                <h3>Payment {{ numero }}</h3>
            </v-col>
            
            <v-col cols="12" md="4">
                <v-text-field v-model="paymentForm.transaction_id" label="Transaction ID" @blur="enviarData"
                    required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="paymentForm.amount" type="number" @blur="enviarData" label="Amount" required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-menu v-model="menu" :close-on-content-click="false" :nudge-right="40" transition="scale-transition"
                    offset-y min-width="auto">
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field v-model="paymentForm.date" label="Date" prepend-icon="mdi-calendar" readonly
                            v-bind="attrs" v-on="on" @blur="enviarData"></v-text-field>
                    </template>
                    <v-date-picker v-model="paymentForm.date" @input="menu = false"></v-date-picker>
                </v-menu>
            </v-col>
        </v-row>

    </div>
</template>

<script>
export default {
    name: "Payment-Register",
    props:[
        'numero'
    ],
    data: () => ({
        menu: false,

        dataPaymentForm: {},

        paymentForm: {
            transaction_id: '',
            amount: '',
            date: '',
        },

        num_pay: [],

    }),
    methods: {
        enviarData() {

            this.dataPaymentForm = {
                payment_number : this.numero,
                payment_form : this.paymentForm
            }

            // console.log(this.dataPaymentForm);
            this.$emit('paymentForm',this.dataPaymentForm);
            this.$emit('insertPayment');
        }
    },
}
</script>