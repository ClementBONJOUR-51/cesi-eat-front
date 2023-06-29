<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-4">
      <div class="flex items-center justify-between mb-8">
        <button @click="goToProfile" class="px-4 py-2 rounded bg-red-500 flex items-center">
          <!-- <img class="h-6 w-6 mr-2" src="../../src/images/burger-king.jpg" alt="Retour au profil"> -->
          <span>Retour au profil</span>
        </button>
        <input
          v-model="searchTerm"
          class="px-4 py-2 border rounded shadow-md ml-4 mr-2 flex-grow"
          type="search"
          placeholder="Rechercher une commande..."
        >
        <!-- <select v-model="filterStatus" class="px-4 ml-2 mr-4 py-2 border rounded shadow-md">
          <option value="">Tous</option>
          <option value="PREPARED">Livrée</option>
        </select> -->
      </div>
      <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
        <div class="order-card bg-yellow-300 rounded p-4" v-for="order in filteredOrders" :key="order._id">
          <h2 class="font-bold mb-2">Commande n° {{ order.invoice_number }}</h2>
          <p class="mb-2">Statut: {{ order.order_state }}</p>
          <p>{{ order.customer.firstname }} {{ order.customer.lastname }}</p>
          <div class="flex justify-end mt-4">
            <button @click="viewOrderDetails(order)" class="px-4 py-2 rounded bg-blue-500 text-white mr-2">
              Détails
            </button>
            <button @click="editOrder(order)" class="px-4 py-2 rounded bg-green-500 text-white mr-2">
              Modifier
            </button>
            <button @click="confirmDeleteOrder(order)" class="px-4 py-2 rounded bg-red-500 text-white">
              Supprimer
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- Boîte de dialogue modale de confirmation de suppression -->
    <div v-if="showDeleteConfirmation" class="fixed inset-0 flex items-center justify-center z-50 bg-black bg-opacity-75">
      <div class="bg-white p-4 rounded shadow-md">
        <h2 class="text-lg font-bold mb-4">Êtes-vous sûr de vouloir supprimer cet article ?</h2>
        <div class="flex justify-end">
          <button @click="deleteOrder(orderToDelete)" class="px-4 py-2 bg-red-500 text-white rounded mr-2">
            Oui
          </button>
          <button @click="cancelDelete" class="px-4 py-2 bg-gray-500 text-white rounded">
            Non
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-else class="w-screen h-screen bg-emerald-500 m-0 flex items-center justify-center flex-col">
    <h1 class="text-white m-4 text-2xl">Veuillez vous connecter pour accéder à cette page.</h1>
    <button @click="logout"
      class="bg-transparent hover:bg-white text-white font-semibold hover:text-white py-2 px-4 border border-white hover:border-transparent rounded">
      Se connecter
    </button>
  </div>
</template>
  
<script>
import axios from 'axios';
import jwt_decode from 'jwt-decode';

export default {
  data() {
    return {
      orders: [],
      searchTerm: '',
      filterStatus: '',
      isAuthenticated: false,
      showDeleteConfirmation: false,
      orderToDelete: null,
    };
  },
  computed: {
    filteredOrders() {
      let filtered = this.orders;

      if (this.searchTerm !== '') {
        filtered = filtered.filter((order) =>
          order.invoice_number.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }

      if (this.filterStatus !== '') {
        filtered = filtered.filter((order) => order.order_state === this.filterStatus);
      }

      filtered = filtered.filter((order) => order.order_state === 'PREPARED');

      return filtered;
    },
  },
  methods: {
    logout() {
      localStorage.removeItem('authToken');
      this.$router.push({ path: '../pages_connexion/connexion' });
    },
    goToProfile() {
      this.$router.push({ path: '../order_page' });
    },
    editOrder(order) {
      // Stocker les données de la commande dans le stockage local
      localStorage.setItem('orderToModify', JSON.stringify(order));
      // Rediriger vers la page de modification de la commande
      this.$router.push({ path: `./modify_order` });
    },
    confirmDeleteOrder(order) {
      this.orderToDelete = order;
      this.showDeleteConfirmation = true;
    },
    async deleteOrder(order) {
      try {
        await axios.delete(`${useRuntimeConfig().public.api_base_url} / deleteOrder / ${order._id}`);
        // Mettre à jour la liste des commandes
        this.orders = this.orders.filter((o) => o._id !== order._id);
      } catch (error) {
        console.error(error);
      }
      this.cancelDelete();
    },
    cancelDelete() {
      this.showDeleteConfirmation = false;
      this.orderToDelete = null;
    },
    viewOrderDetails(order) {
      // Rediriger vers la page de détails de la commande en utilisant l'ID de la commande
      this.$router.push({ path: `./ order_details / ${order._id}` });
    },
  },
  async created() {
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const userId = decoded.id;
        try {
          const check = await axios.get(`${useRuntimeConfig().public.api_base_url}/getUser/${userId}`);
          this.isAuthenticated = true;

          const resto = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRestorantByRestorerId/${userId}`);
          const restoId = resto.data.result._id;
          localStorage.setItem('restaurantId', restoId);

          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getOrdersByRestorantId/${restoId}`);
          this.orders = response.data.result.orders;
        } catch (error) {
          console.error(error);
        }
      }
    }
  },
};
</script>
  
<style scoped>
.order-card {
  box-shadow: 0 0.5em 1em -0.125em rgb(10 10 10 / 10%), 0 0 0 1px rgb(10 10 10 / 2%);
}
</style>
  
