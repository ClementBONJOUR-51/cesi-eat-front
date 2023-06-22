<template>
    <div class="flex justify-center items-start flex-col md:flex-row md:justify-between md:items-start flex-wrap h-screen">
        <div class="md:w-3/4 overflow-auto">
            <h1 class="bg-emerald-600 text-white text-xl md:text-2xl p-2 text-center md:text-left">Restaurant -
                <b>{{ restaurant.restorant_name }}</b>
            </h1>
            <div v-if="isLoading">Loading...</div>
            <div v-else class="flex flex-wrap p-2">
                <div v-for="product in products" :key="product._id"
                    class="bg-emerald-400 max-w-xs rounded-[15px] shadow-lg mb-4 mr-4">
                    <div class="px-0 py-0 grid grid-cols-1 gap-2">
                        <div class="col-span-full">
                            <img class="rounded-[15px] w-full h-32 object-cover" :src="'https://picsum.photos/300/200'" />
                        </div>
                        <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                            <div class="flex items-center">
                                {{ product.product_name }}
                            </div>
                        </div>
                        <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                            <div>
                                <small>{{ product.product_category + " - " + product.product_price }} €</small>
                            </div>
                        </div>
                        <div class="w-full px-4">
                            <button @click="addToCart(product)"
                                class="bg-white hover:bg-emerald-600 hover:text-white font-bold mb-2 py-2 px-4 rounded-full w-full">
                                Ajouter
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="md:w-1/4 flex flex-col bg-slate-50">
            <h1 class="text-xl md:text-2xl p-2 bg-emerald-500 text-white text-center md:text-left">Panier</h1>
            <div id="panier" class="p-2 overflow-auto flex-1">
                <div class="flex flex-col space-y-2">
                    <div v-for="(product, index) in cart" :key="index" class="flex justify-between">
                        <div class="font-bold text-sm md:text-lg">
                            {{ product.product_name }}
                        </div>
                        <div class="text-sm md:text-lg">
                            {{ product.product_price }} €
                        </div>
                    </div>
                    <hr class="my-2">
                    <div class="font-bold text-lg flex justify-between">
                        <div>Total:</div>
                        <div>{{ totalSum }} €</div>
                    </div>
                </div>
            </div>
            <div class="mt-4 p-2">
                <form @submit.prevent="submitOrder">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mt-4">
                        <div>
                            <label class="block">Numéro de rue:</label>
                            <input v-model="order.address.street_number" type="number" required
                                class="w-full mt-1 border border-black" />
                        </div>
                        <div>
                            <label class="block">Rue:</label>
                            <input v-model="order.address.street" type="text" required
                                class="w-full mt-1 border border-black" />
                        </div>
                        <div>
                            <label class="block">Code Postal:</label>
                            <input v-model="order.address.postal_code" type="text" required
                                class="w-full mt-1 border border-black" />
                        </div>
                        <div>
                            <label class="block">Ville:</label>
                            <input v-model="order.address.city" type="text" required
                                class="w-full mt-1 border border-black" />
                        </div>
                    </div>

                    <button type="submit"
                        class="mt-4 bg-emerald-500 hover:bg-emerald-700 text-white font-bold py-2 px-4 rounded w-full">
                        Commander
                    </button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";

export default {
    data() {
        return {
            isLoading: true,
            restaurant: {},
            products: [],
            cart: [],
            order: {
                restorant: "",
                order_state: "En cours",
                paid: 0,
                products: [],
                customer: {
                    id_customer: "",
                    firstname: "",
                    lastname: "",
                    gender: "",
                    birthday: "",
                    phone: "",
                    email: ""
                },
                address: {
                    street: "",
                    postal_code: "",
                    city: "",
                    street_number: null,
                    lati: 0,
                    longi: 0
                },
                discount: 5,
            },
        };
    },
    computed: {
        totalSum() {
            return this.cart.reduce((sum, product) => sum + product.product_price, 0);
        },
    },
    methods: {
        addToCart(product) {
            this.cart.push(product);
        },
        async submitOrder() {
            this.order.restorant = this.restaurant._id;
            this.order.products = this.cart.map((product) => ({ id_product: product._id }));

            try {
                const response = await axios.post("http://localhost:3000/createOrder", this.order);
                console.log(response.data);
                alert("Commande effectuée avec succès !")
                this.$router.push('/Acceuil')
            } catch (error) {
                console.error(error);
                alert("Erreur lors de la commande !")
            }
        },
    },
    async mounted() {
        try {
            const token = localStorage.getItem('authToken');
            if (token) {
                const decodedToken = jwt_decode(token);
                console.log(decodedToken);
                this.order.customer = {
                    id_customer: decodedToken.id,
                    firstname: decodedToken.firstname,
                    lastname: decodedToken.lastname,
                    gender: decodedToken.gender,
                    birthday: decodedToken.birthday,
                    phone: decodedToken.phone,
                    email: decodedToken.email
                };
            } else {
                alert("TOKEN NOT FOUND")
            }

            const idResto = this.$route.params.id;
            const response = await axios.get(`http://localhost:3000/getAllProductsFromRestorant/${idResto}`);
            this.restaurant = response.data.result.products[0].restorant;
            this.products = response.data.result.products;
            this.isLoading = false;
        } catch (error) {
            console.error(error);
        }
    },
};
</script>
