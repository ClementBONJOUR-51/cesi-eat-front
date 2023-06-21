<template>
    <div v-bind:key="order" v-for="order in orders">
        <components_customersCard-orders :order="order"/>
    </div>
</template>

<script>
import axios from 'axios';
export default {
  async mounted() {
    try {
      const response = await axios.get('http://localhost:3000/getOrdersWithoutDeliveryPerson');
      this.orders = response.data.result.orders;
      console.log(this.orders);
    } catch (error) {
      console.error(error);
    }
  }
};
</script>
