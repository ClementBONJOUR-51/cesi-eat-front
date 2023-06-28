<template>
  <div>
    <div v-if="isAuthenticated" class="container mx-auto p-4">
      <div class="grid md:grid-cols-2 gap-4">
        <button
          v-for="(button, index) in buttons"
          :key="index"
          @click="button.action"
          :class="button.class"
        >
          {{ button.label }}
        </button>
      </div>
    </div>
    <div v-else class="container mx-auto p-4">
      <p>Veuillez vous connecter pour accéder à cette page.</p>
      <button @click="logout" class="py-2 px-4 bg-blue-500 text-white rounded">
        Se connecter
      </button>
    </div>
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
      isAuthenticated: false,
      buttons: [
        {
          label: 'Commande à accepter',
          class: 'py-8 px-4 bg-emerald-800 text-white rounded',
          action: () => this.goToOrderConfirmation(),
        },
        {
          label: 'Commande en cours',
          class: 'py-8 px-4 bg-emerald-700 text-white rounded',
          action: () => this.goToOrderInProgress(),
        },
        {
          label: 'Commande livrée',
          class: 'py-8 px-4 bg-emerald-600 text-white rounded',
          action: () => this.goToOrderList(),
          
        },
        {
          label: 'Retourner au profil',
          class: 'py-8 px-4 bg-red-500 text-white rounded',
          action: () => this.returnToProfil(),
        },
      ],
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
  
