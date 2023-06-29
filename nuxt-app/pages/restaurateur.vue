<template>
  <div>
    <div class="mb-4 text-center" v-if="user">
      <h1 class="p-2 text-2xl font-bold text-gray-800 mb-2">Bienvenue, {{ user.firstname }}</h1>
      <p class="text-gray-600">Votre adresse e-mail est : {{ user.email }}</p>
    </div>
    <div v-if="isAuthenticated" class="container mx-auto p-4">
      <div class="grid md:grid-cols-3 gap-4">
        <button v-for="(button, index) in buttons" :key="index" @click="button.action" :class="button.class">
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
import jwt_decode from 'jwt-decode';

export default {
  data() {
    return {
      user: null,
      loading: true,
      isAuthenticated: false,
      buttons: [
        {
          label: 'Voir les commandes',
          action: () => this.goToOrder(),
          class: 'py-8 px-4 bg-emerald-800 text-white rounded',
        },
        {
          label: 'Voir les menus',
          action: () => this.goToMenus(),
          class: 'py-8 px-4 bg-emerald-700 text-white rounded',
        },
        {
          label: 'Voir les articles',
          action: () => this.goToProducts(),
          class: 'py-8 px-4 bg-emerald-600 text-white rounded',
        },
        {
          label: 'Changer le profil',
          action: () => this.goToChangeProfil(),
          class: 'py-8 px-4 bg-emerald-500 text-white rounded',
        },
        {
          label: 'Statistiques',
          action: () => this.goToStat(),
          class: 'py-8 px-4 bg-emerald-400 text-white rounded',
        },
        {
          label: 'Se déconnecter',
          action: () => this.logout(),
          class: 'py-8 px-4 bg-emerald-300 text-white rounded',
        },
        {
          label: 'Supprimer le compte',
          action: () => this.confirmDelete(),
          class: 'py-8 px-4 bg-red-500 text-white rounded',
        },
      ],
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
      this.$router.push({ path: './modify/restaurateur/modify_profil_restaurateur' });
    },
    goToStat() {
      this.$router.push({ path: `./restaurant/stats/${restaurant._id}` });
    },
    logout() {
      localStorage.removeItem('authToken');
      this.$router.push({ path: './pages_connexion_inscription/pages_connexion/connexion' });
    },
    async deleteUser() {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const userId = decoded.id;
        try {
          await axios.delete(`${useRuntimeConfig().public.api_base_url}deleteUser/${userId}`);
          this.logout();  // Se déconnecter après la suppression du compte
        } catch (error) {
          console.error(error);
        }
      }
    },
    confirmDelete() {
      if (
        window.confirm(
          'Êtes-vous sûr de vouloir supprimer votre compte ? Cette action est irréversible.'
        )
      ) {
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
          const restaurant_response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRestorantByRestorerId/${userId}`);
          const restaurant = restaurant_response.data;
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
  
