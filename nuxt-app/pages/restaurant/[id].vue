<template>
    <div>
        <h1>Restaurant {{ restaurant.restorant_name }}</h1>
        <h2>Products:</h2>
        <div v-if="isLoading">Loading...</div>
        <ul v-else>
            <li v-for="product in products" :key="product._id">
                {{ product.product_name }} - {{ product.product_price }}
            </li>
        </ul>
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
        };
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
  