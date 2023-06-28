<template>
    <div class="bg-emerald-400 h-screen flex flex-col justify-end">
        <div class="flex items-left justify-center h-[25%] flex-col">
            <div class="m-5">
                <composants_generiquePrevButton />
            </div>
            <span class="text-2xl m-5">Suivi de ma commande</span>
        </div>
        <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px] gap-1 flex flex-col items-center ">
            <div v-for="(currentOrder, index) in currentOrders" :key="index">
                <div class="bg-emerald-400 max-w-xs rounded-[15px] mt-5 shadow-lg p-5">
                    <div v-if="currentOrder" class="gap-1 flex flex-col items-center justify-center">
                        <div class="text-2xl my-2">Information commande:</div>
                        <div class="my-2 mb-2" v-if="currentOrder.delivery_person">
                            Livreur : {{ currentOrder.delivery_person.firstname }} {{ currentOrder.delivery_person.lastname
                            }}
                        </div>
                        <div class="my-2 mb-2" v-else>
                            Livreur : Non assigné
                        </div>
                        <div class="my-2 mb-2">
                            Numéro de commande : {{ currentOrder.invoice_number }}
                        </div>
                        <div class="text-2xl my-2">Etat de la commande:</div>
                        <div class="my-2">
                            <div class="mb-2">
                                <span
                                    v-if="['CREATED', 'PAID', 'PREPARED', 'TAKEN', 'RACING', 'DELIVERED'].includes(currentOrder.order_state)">✔️</span>
                                <span v-else>❌</span> Commande passée
                            </div>
                            <div class="mb-2">
                                <span
                                    v-if="['PREPARED', 'TAKEN', 'RACING', 'DELIVERED'].includes(currentOrder.order_state)">✔️</span>
                                <span v-else>❌</span> En cours de préparation
                            </div>
                            <div class="mb-2">
                                <span v-if="['TAKEN', 'RACING', 'DELIVERED'].includes(currentOrder.order_state)">✔️</span>
                                <span v-else>❌</span> En cours de livraison
                            </div>
                            <div class="mb-2">
                                <span v-if="['DELIVERED'].includes(currentOrder.order_state)">✔️</span>
                                <span v-else>❌</span> Livré
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <composants_generiqueMobileNavBar class="md:hidden" />
    </div>
</template>


<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";
export default {
    data() {
        return {
            currentOrders: []
        }
    },
    async created() {
        const token = localStorage?.getItem('authToken');
        const decoded = jwt_decode(token);
        const id_customer = decoded.id;
        console.log(id_customer);
        try {
            const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getOrdersByCustomerId/${id_customer}`);
            console.log("Response data: ", response.data.result);
            console.log(typeof response.data.result, response.data.result);

            this.currentOrders = response.data.result.orders.filter(order => order.order_state !== 'DELIVERED');

            console.log(this.currentOrders, "test2");
        } catch (error) {
            console.error(error);
        }
    },
    methods: {
        checkAuthentication() {
            const token = localStorage.getItem('authToken');
            console.log(token);
            if (!token) {
                this.redirectToLoginPage();
            }
        },
        redirectToLoginPage() {
            this.$router.push({ path: '../pages_connexion_inscription/pages_connexion/connexion' });
        }
    },
    mounted() {
        this.checkAuthentication();
    }
}
</script>

<style scoped>
@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

.animate {
    animation: spin 2s linear infinite;
}
</style>

