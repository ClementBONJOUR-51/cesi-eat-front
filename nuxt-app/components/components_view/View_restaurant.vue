<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <div class="grid gap-4">
      <button @click="goToOrder" class="py-2 px-4 bg-purple-500 text-white rounded">
        Voir les commandes
      </button>
      <button @click="goToMenus" class="py-2 px-4 bg-blue-500 text-white rounded">
        Voir les menus
      </button>
      <button @click="goToProducts" class="py-2 px-4 bg-green-500 text-white rounded">
        Voir les articles
      </button>
      <button @click="goToChangeProfil" class="py-2 px-4 bg-yellow-500 text-white rounded">
        Changer le profil
      </button>
      <button @click="logout" class="py-2 px-4 bg-red-500 text-white rounded">
        Se déconnecter
      </button>
      <button @click="confirmDelete" class="py-2 px-4 bg-red-500 text-white rounded">
        Supprimer le compte
      </button>
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
    goToOrder() {
      this.$router.push({ path: './order/restaurateur/order_page' });
    },
    goToMenus() {
      this.$router.push({ path: './menu/list_menu' });
    },
    goToProducts() {
      this.$router.push({ path: './product/list_product' });
    },
    goToChangeProfil() {
      this.$router.push({ path: './modify/restaurateur/modify_profil' });
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
