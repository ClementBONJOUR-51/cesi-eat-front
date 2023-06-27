<template>
    <div>
        <div class="mb-4" v-if="user">
            <h1 class="text-xl font-bold">Bienvenue, {{ user.firstname }}</h1>
            <p class="text-gray-700">Votre adresse e-mail est : {{ user.email }}</p>
        </div>
        <div v-if="isAuthenticated" class="container mx-auto p-4">
        <div class="grid gap-4">
            <button
            @click="goToOrder"
            class="py-2 px-4 bg-purple-500 text-white rounded"
            >
            Voir les commandes
            </button>
            <button
            @click="goToMenus"
            class="py-2 px-4 bg-blue-500 text-white rounded"
            >
            Voir les menus
            </button>
            <button
            @click="goToProducts"
            class="py-2 px-4 bg-green-500 text-white rounded"
            >
            Voir les articles
            </button>
            <button
            @click="goToChangeProfil"
            class="py-2 px-4 bg-yellow-500 text-white rounded"
            >
            Changer le profil
            </button>
            <button
            @click="logout"
            class="py-2 px-4 bg-red-500 text-white rounded"
            >
            Se déconnecter
            </button>
            <button
            @click="confirmDelete"
            class="py-2 px-4 bg-red-500 text-white rounded"
            >
            Supprimer le compte
            </button>
        </div>
        </div>
        <div v-else class="container mx-auto p-4">
        <p>Veuillez vous connecter pour accéder à cette page.</p>
        <button
            @click="logout"
            class="py-2 px-4 bg-blue-500 text-white rounded"
        >
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
            await axios.delete(`http://localhost:3000/deleteUser/${userId}`);
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
            const response = await axios.get(`http://localhost:3000/getUser/${userId}`);
            this.user = response.data.result;
            console.log(this.user);
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
  