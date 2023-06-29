<template>
  <div v-if="orders.length > 0" v-bind:key="order" v-for="order in orders">
      <components_customersCard-orders-delivered :order="order" :fetchData="fetchData" :removeOrder="removeOrder" />
  </div>
  <div v-else class="flex items-center justify-center h-screen flex-col bg-emerald-700 text-white space-y-4">
      <p class="text-lg">Il n'y a aucune commande livrées.</p>
      <button @click="goBack" class="py-2 px-4 rounded bg-white text-emerald-700 font-semibold hover:bg-emerald-500 hover:text-white transition-colors duration-200">Retour</button>
  </div>
</template>
  
  <script>
  import axios from 'axios';
  import { onMounted, ref } from 'vue';
  import jwt_decode from "jwt-decode";

  export default {
    name: 'Livreur',
    setup() {
      const orders = ref([]);
  
      const fetchData = async () => {
        try {
        const token = localStorage.getItem('authToken');
        const decoded_delivery_id = jwt_decode(token);
          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getOrdersByDeliveryId/${decoded_delivery_id.id}`);
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
    methods: {
      goBack() {
      this.$router.push({ path: '../../profil_livreur' });
    },
  },
  }
  </script>