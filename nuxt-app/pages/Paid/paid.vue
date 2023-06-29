<template>
  <div class="flex flex-col items-center justify-center min-h-screen text-xl">
    <composants_generiqueNavBar class="sticky"/>
    <div v-if="orders.length > 0" v-bind:key="order" v-for="order in orders">
      <components_customersCard-orders-notpaid :order="order" :fetchData="fetchData" :removeOrder="removeOrder" />
    </div>
    <div v-else class="text-center">
      <h3 class="mb-4">Plus de commande à payer</h3>
      <button class="bg-emerald-400 text-white px-4 py-2 rounded" @click="goToHomePage">Accueil</button>
    </div>
    <composants_generiqueMobileNavBar class="md:hidden"/>
  </div>
</template>

  
  <script>
  import axios from 'axios';
  import { onMounted, ref } from 'vue';
  import jwt_decode from "jwt-decode";
  
  export default {
    name: 'Paid',
    methods: {
  // vos autres méthodes
      goToHomePage() {
        this.$router.push({  path: "../Accueil" }); // remplacer 'Home' par le nom correct de votre route d'accueil
      }
    },
    setup() {
      const orders = ref([]);
  
      const fetchData = async () => {
        try {
        const token = localStorage.getItem('authToken');
        const decoded_user_id = jwt_decode(token);
          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getOrdersNotPaidByCustomerId/${decoded_user_id.id}`);
          return response.data.result.orders;
        } catch (error) {
          console.error(error);
          return [];
        }
      };
  
      const removeOrder = (orderId) => {
        orders.value = orders.value.filter(order => order._id !== orderId);
      };
  
      onMounted(async () => {
        orders.value = await fetchData();
      });
  
      return {
        orders,
        fetchData,
        removeOrder,
      };
    },
  }
  </script>