<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <div class="grid gap-4">
      <button @click="goToOrderConfirmation" class="py-2 px-4 bg-blue-500 text-white rounded">
        Commande à accepter
      </button>
      <button @click="goToOrderInProgress" class="py-2 px-4 bg-green-500 text-white rounded">
        Commande en cours
      </button>
      <button @click="goToOrderList" class="py-2 px-4 bg-yellow-500 text-white rounded">
        Commande livré
      </button>
      <button @click="returnToProfil" class="py-2 px-4 bg-red-500 text-white rounded">
        Retourner au profil
      </button>
    </div>
  </div>
  <div v-else class="container mx-auto p-4">
    <p>Veuillez vous connecter pour accéder à cette page.</p>
    <button @click="logout" class="py-2 px-4 bg-blue-500 text-white rounded">
      Se connecter
    </button>
  </div>
</template>
  
<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";

export default {
  data() {
    return {
      user: null,
      loading: true,
      isAuthenticated: false
    };
  },
  methods: {
    goToOrderConfirmation() {
      this.$router.push({ path: './order/order_confirmation' });
    },
    goToOrderInProgress() {
      this.$router.push({ path: './order/order_inprogress' });
    },
    goToOrderList() {
      this.$router.push({ path: './order/order_list' });
    },
    returnToProfil() {
      this.$router.push({ path: '../../restaurateur' });
    },
    logout() {
      localStorage.removeItem('authToken');
      this.$router.push({ path: './pages_connexion/connexion' });
    },
    async deleteUser() {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const userId = decoded.id;
        try {
          await axios.delete(`${useRuntimeConfig().public.api_base_url}/deleteUser/${userId}`);
          this.logout();  // Se déconnecter après la suppression du compte
        } catch (error) {
          console.error(error);
        }
      }
    },
    confirmDelete() {
      if (window.confirm('Êtes-vous sûr de vouloir supprimer votre compte ? Cette action est irréversible.')) {
        this.deleteUser();
      }
    },
  },
  async created() {
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const userId = decoded.id;
        try {
          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getUser/${userId}`);
          this.user = response.data.result;
          this.loading = false;
          this.isAuthenticated = true;
        } catch (error) {
          console.error(error);
          this.loading = false;
        }
      }
    }
  },
};
</script>
  