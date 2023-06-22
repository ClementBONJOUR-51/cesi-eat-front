<template>
    <div class="flex justify-center items-start flex-col md:flex-row md:justify-between md:items-start flex-wrap h-screen">
        <div class="md:w-3/4 overflow-auto">
            <h1 class="bg-emerald-600 text-white text-xl md:text-2xl p-2 text-center md:text-left">Restaurant {{
                restaurant.restorant_name }}</h1>
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
                                Add to cart
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="md:w-1/4 flex flex-col">
            <h1 class="text-xl md:text-2xl p-2 bg-emerald-500 text-white text-center md:text-left">Panier</h1>
            <div id="panier" class="bg-white p-2 overflow-auto flex-1">
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
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            isLoading: true,
            restaurant: {},
            products: [],
            cart: [],
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
    },
    async mounted() {
        try {
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
