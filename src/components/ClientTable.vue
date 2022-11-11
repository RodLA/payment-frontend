<template>
    <div>
        <v-row>
            <v-col cols='12' md="12" >
                <div class="d-flex justify-center" ><h1>Client list</h1></div>
            </v-col>
        </v-row>
        <div class="d-flex flex-row-reverse mb-4">
            <v-btn @click="registerAddress" color="primary" >
                Register new client 
                <v-icon dark right>
                    mdi-arrow-right
                </v-icon>
            </v-btn>
        </div>
        <div>
            <v-data-table :headers="headers" :items="clientList" :items-per-page="10" class="elevation-1"></v-data-table>
        </div>
    </div>
</template>
<script>

import axios from 'axios';
export default {
    name: 'Client-Table',
    data() {
        return {
            headers: [
                { text: 'First name', value: 'first_name' },
                { text: 'Last name', value: 'last_name' },
                { text: 'DOB', value: 'dob' },
                { text: 'Phone', value: 'phone' },
                { text: 'Email', value: 'email' },
                { text: 'Address', value: 'address' },
                { text: 'Nro. Payments', value: 'number_payments' },
                { text: 'Total', value: 'total_payments' },
            ],
            clientList: [],
        }
    },
    mounted() {
        this.getClients();
    },
    methods: {
        getClients() {
            axios
                .get('/user/payment/all')
                .then((response) => {
                    this.clientList = response.data;
                })
                .catch((response) => {
                    console.log(response);
                });
        },
        registerAddress() {
            this.$router.push({ name: 'register' });
        }
    },

}
</script>