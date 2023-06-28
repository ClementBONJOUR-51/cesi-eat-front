<template>
    <div>
      <div class="mb-4 text-center" v-if="user">
        <h1 class="p-2 text-2xl font-bold text-gray-800 mb-2">Bienvenue, {{ user.firstname }}</h1>
        <p class="text-gray-600">Votre adresse e-mail est : {{ user.email }}</p>
      </div>
      <div v-if="isAuthenticated" class="container mx-auto p-4">
        <div class="grid md:grid-cols-2 gap-4">
          <button v-for="(button, index) in buttons" :key="index" @click="button.action" :class="button.class">
            {{ button.label }}
          </button>
        </div>
      </div>
      <div v-else class="w-screen h-screen bg-emerald-500 m-0 flex items-center justify-center flex-col ">
    <h1 class="text-white m-4 sm:text-2xl text-sm">Veuillez vous connecter pour accéder à cette page.</h1>
    <button @click="logout"
      class="bg-transparent hover:bg-white text-white font-semibold hover:text-emerald-500 py-2 px-4 border border-white hover:border-transparent rounded">
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
            label: 'Liste des commandes',
            action: () => this.goToLCommandeLivreur(),
            class: 'py-8 px-4 bg-emerald-700 text-white rounded',
          },
          {
            label: 'Historique des commandes livrées',
            action: () => this.goToCommandeDelivered(),
            class: 'py-8 px-4 bg-emerald-600 text-white rounded',
          },
          {
            label: 'Modifier profil',
            action: () => this.goToProfil(),
            class: 'py-8 px-4 bg-emerald-500 text-white rounded',
          },
          {
            label: 'Se déconnecter',
            action: () => this.logout(),
            class: 'py-8 px-4 bg-emerald-400 text-white rounded',
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
        goToLCommandeLivreur() {
        this.$router.push({ path: './livreur/commande_livreur' });
      },
      goToCommandeDelivered() {
        this.$router.push({ path: './livreur/commande_delivered' });
      },
      goToProfil() {
        this.$router.push({ path: './modify/livreur/modify_profil_livreur' });
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
          console.log(decoded, "test");
          const userId = decoded.id;
          console.log(userId, "test");
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
    
  