<template>
  <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
    <div class="bg-emerald-400 rounded p-4">
      <h2 class="font-bold mb-2">Numéro de commande : {{ order.invoice_number }}</h2>
      <h2 class="font-bold mb-2">Destinataire</h2>
      <p class="mb-2">{{ order.customer.firstname }} {{ order.customer.lastname }}</p> <!-- Ou si pb mettre || '' -->
      <p class="mb-2">{{ order.customer.phone }}</p>
      <h2 class="font-bold mb-2">Adresse de livraison</h2>
      <p>{{ order.address.street_number }} {{ order.address.street }}</p>
      <p class="mb-2">{{ order.address.postal_code }} {{ order.address.city }}</p>
      <h2 class="font-bold mb-2">Restaurant</h2>
      <p>{{ order.restorant.restorant_name }}</p>
      <p class="mb-2">{{ order.restorant.phone_number }}</p>
      <p>{{ order.restorant.address.street_number }} {{ order.restorant.address.street }}</p>
      <p class="mb-2">{{ order.restorant.address.postal_code }} {{ order.restorant.address.city }}</p>
      <div class="flex justify-end mt-4">
        <button @click="acceptOrder(order)" class="px-4 py-2 rounded bg-green-500 text-white mr-2">
          Accepter
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import jwt_decode from "jwt-decode";

export default {
  name: 'Card',
  props: {
    order: {
      type: Object,
      required: true
    },
    fetchData: {
      type: Function,
      required: true
    },
    removeOrder: {
      type: Function,
      required: true
    }
  },
  methods: {
    async acceptOrder(order) {
      try {
        const token = localStorage.getItem('authToken');
        const decoded_user_id = jwt_decode(token);

        const response = await axios.put(`http://localhost:3000/assignDeliveryPersonToOrder/${order._id}`, {
          delivery_person: {
            id_delivery_person: decoded_user_id.id,
            firstname: decoded_user_id.firstname,
            lastname: decoded_user_id.lastname,
            email: decoded_user_id.email,
            phone_delivery: decoded_user_id.phone,
          },
        
        });

        if (response.data.status !== 'success') {
          alert(response.data.message);
        } else {
          alert('Commande acceptée !');
          this.removeOrder(order._id);
        }
      } catch (error) {
        console.error(error);
      }
    },
  }
}
</script>

