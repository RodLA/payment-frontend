<template>
    <div>
        <div class="d-flex flex-row-reverse">
            <v-btn @click="registerAddress">
                Register client
            </v-btn>
        </div>
        <div>
            <v-data-table :headers="headers" :items="clientList" :items-per-page="5" class="elevation-1"></v-data-table>
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