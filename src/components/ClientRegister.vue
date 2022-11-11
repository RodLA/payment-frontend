<template>
    <div>
        <v-row class="">
            <v-col cols="12" md="12">
                <div class="d-flex flex-row-reverse">
                    <v-btn @click="homeAddress" color="primary">
                        <v-icon dark left>
                            mdi-arrow-left
                        </v-icon> Table client
                    </v-btn>
                </div>
            </v-col>
        </v-row>

        <v-form ref="form" v-model="valid" lazy-validation>
            <v-row>
                <v-col cols="12" md="12">
                    <h1>Client register</h1>
                </v-col>
            </v-row>
            <v-row>
                <v-col cols="12" md="4">
                    <v-text-field v-model="userPaymentData.user.first_name" :rules="stringRules" label="First name"
                        required>
                    </v-text-field>
                </v-col>

                <v-col cols="12" md="4">
                    <v-text-field v-model="userPaymentData.user.last_name" :rules="stringRules" label="Last name"
                        required>
                    </v-text-field>
                </v-col>

                <v-col cols="12" md="4">

                    <v-menu v-model="menu" :close-on-content-click="false" :nudge-right="40"
                        transition="scale-transition" offset-y min-width="auto">
                        <template v-slot:activator="{ on, attrs }">
                            <v-text-field v-model="userPaymentData.user.dob" label="DOB" prepend-icon="mdi-calendar"
                                readonly v-bind="attrs" v-on="on" :rules="stringRules"></v-text-field>
                        </template>
                        <v-date-picker v-model="userPaymentData.user.dob" @input="menu = false"></v-date-picker>
                    </v-menu>

                </v-col>

                <v-col cols="12" md="4">
                    <v-text-field v-model="userPaymentData.user.phone" :rules="numberRules" type="number" label="Phone"
                        required>
                    </v-text-field>
                </v-col>

                <v-col cols="12" md="4">
                    <v-text-field v-model="userPaymentData.user.email" :rules="emailRules" type="email" label="Email"
                        required>
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
                    <v-text-field v-model="userPaymentData.payments[index].transaction_id" :rules="stringRules"
                        label="Transaction ID" required>
                    </v-text-field>
                </v-col>

                <v-col cols="12" md="4">
                    <v-text-field v-model="userPaymentData.payments[index].amount" :rules="numberRules" type="number"
                        label="Amount" required>
                    </v-text-field>
                </v-col>

                <v-col cols="12" md="4">
                    <v-menu v-model="menuArray[index]" :close-on-content-click="false" :nudge-right="40"
                        transition="scale-transition" offset-y min-width="auto">
                        <template v-slot:activator="{ on, attrs }">
                            <v-text-field v-model="userPaymentData.payments[index].date" label="Date"
                                prepend-icon="mdi-calendar" readonly v-bind="attrs" v-on="on" :rules="stringRules">
                            </v-text-field>
                        </template>
                        <v-date-picker v-model="userPaymentData.payments[index].date" @input="menuArray[index] = false">
                        </v-date-picker>
                    </v-menu>
                </v-col>

            </v-row>

            <v-row>
                <v-col cols="12" md="12">
                    <div class="d-flex flex-row-reverse">
                        <v-btn class="mx-2" fab dark color="primary" @click="addPayment">
                            <v-icon dark>
                                mdi-plus
                            </v-icon>
                        </v-btn>
                    </div>
                </v-col>
            </v-row>

            <v-row>
                <v-col cols="12" md="12">
                    <v-btn color="primary" @click="register()">
                        Save
                    </v-btn>
                </v-col>
            </v-row>
        </v-form>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "Client-Register",
    components: {
    },
    data: () => ({
        valid: true,
        menu: false,
        menuArray: [],
        stringRules: [
            v => !!v || 'Is required'
        ],
        emailRules: [
            v => !!v || 'E-mail is required',
            v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
        ],
        numberRules: [
            v => !!v || 'E-mail is required',
            v => Number.isInteger(Number(v)) || 'The value must be an integer number',
            v => v > 0 || 'The value must be greater than zero'
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
        async register() {

            if (this.$refs.form.validate()) {
                console.log(this.userPaymentData);
                await axios
                    .post('/user/payment/save', this.userPaymentData)
                    .then((response) => {
                        if (response.data.status) {
                            this.$router.push({
                                name: 'home'
                            })
                        }
                    })
                    .catch((response) => {
                        console.log(response);
                    });
            }
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
        homeAddress() {
            this.$router.push({ name: 'home' });
        }
    },
}
</script>
<style>

</style>