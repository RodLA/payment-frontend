<template>
    <div>
        <v-row>
            <v-col cols="12" md="12">
                <h1>Client register</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.user.first_name" :rules="stringRules" label="First name" required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.user.last_name" :rules="stringRules" label="Last name" required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">

                <v-menu v-model="menu" :close-on-content-click="false" :nudge-right="40" transition="scale-transition"
                    offset-y min-width="auto">
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field v-model="userPaymentData.user.dob" label="DOB" prepend-icon="mdi-calendar" readonly
                            v-bind="attrs" v-on="on" :rules="stringRules"></v-text-field>
                    </template>
                    <v-date-picker v-model="userPaymentData.user.dob" @input="menu = false"></v-date-picker>
                </v-menu>

            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.user.phone" :rules="stringRules" label="Phone" required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.user.email" :rules="emailRules" type="email" label="Email" required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.user.address" :rules="stringRules" label="Address" required>
                </v-text-field>
            </v-col>

        </v-row>

        <v-row>
            <v-col cols="12" md="12">
                <h1>Payment register</h1>
            </v-col>
        </v-row>

        <v-row v-for="( payment, index ) in userPaymentData.payments" :key="index" class="payment-space">
            <v-col cols="12" md="12">
                <h3>Payment {{ index + 1 }}</h3>
            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.payments[index].transaction_id" :rules="stringRules" label="Transaction ID"
                    required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-text-field v-model="userPaymentData.payments[index].amount" :rules="stringRules" type="number" label="Amount"
                    required>
                </v-text-field>
            </v-col>

            <v-col cols="12" md="4">
                <v-menu v-model="menuArray[index]" :close-on-content-click="false" :nudge-right="40"
                    transition="scale-transition" offset-y min-width="auto">
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field v-model="userPaymentData.payments[index].date" label="Date" prepend-icon="mdi-calendar" readonly
                            v-bind="attrs" v-on="on" :rules="stringRules"></v-text-field>
                    </template>
                    <v-date-picker v-model="userPaymentData.payments[index].date" @input="menuArray[index] = false"></v-date-picker>
                </v-menu>
            </v-col>

        </v-row>

        <v-row>
            <v-col cols="12" md="12">
                <div class="d-flex flex-row-reverse">
                    <v-btn class="rounded-circle" color="blue" @click="addPayment">+</v-btn>
                </div>
            </v-col>
        </v-row>

        <v-row>
            <v-col cols="12" md="12">
                <v-btn color="blue" @click="register()">Save</v-btn>
            </v-col>
        </v-row>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "Client-Register",
    components: {
    },
    data: () => ({
        menu: false,
        menuArray: [],
        stringRules: [
            v => !!v || 'Is required'
        ],
        emailRules: [
            v => !!v || 'E-mail is required',
            v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],
        
        userPaymentData: {
            user: {
                first_name: '',
                last_name: '',
                dob: '',
                phone: '',
                email: '',
                address: '',
            },
            payments: [],
        }
    }),
    mounted() {
        this.addPayment();
    },
    methods: {
        register() {
            
            console.log( this.userPaymentData );
            axios
                .post('/user/payment/save', this.userPaymentData)
                .then((response) => {
                    console.log(response);
                })
                .catch( (response)=>{
                    console.log(response);
                } );
        },
        addPayment() {
            let newPayment = {
                transaction_id: '',
                amount: '',
                date: '',
            };
            this.userPaymentData.payments.push(newPayment);
            this.menuArray.push(false);
        },



    },
}
</script>
<style>

</style>