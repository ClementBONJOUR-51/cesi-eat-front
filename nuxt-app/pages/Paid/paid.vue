<template>
    <div v-bind:key="order" v-for="order in orders">
      <components_customersCard-orders :order="order" :fetchData="fetchData" :removeOrder="removeOrder" />
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { onMounted, ref } from 'vue';
  
  export default {
    name: 'Livreur',
    setup() {
      const orders = ref([]);
  
      const fetchData = async () => {
        try {
          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getOrdersNotPaidByCustomerId`);
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