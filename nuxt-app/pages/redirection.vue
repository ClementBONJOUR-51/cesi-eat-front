<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <navbarNavbar />
    <div class="mb-4">
      <h1 class="text-xl font-bold">Bienvenue, {{ user.firstname }}</h1>
      <p class="text-gray-700">Votre adresse e-mail est : {{ user.email }}</p>
    </div>
    <div class="">
      <div v-if="isAuthenticated">

      </div>
    </div>
  </div>
  <div v-else class="w-screen h-screen bg-emerald-500 m-0 flex items-center justify-center flex-col">
    <h1 class="text-white m-4 sm:text-2xl text-sm">Veuillez vous connecter pour accéder à cette page.</h1>
    <button @click="logout"
      class="bg-transparent hover:bg-white text-white font-semibold hover:text-emerald-500 py-2 px-4 border border-white hover:border-transparent rounded">
      Se connecter
    </button>
  </div>
</template>
  
    
<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";
import View_restaurant from '../components/components_view/View_restaurant';
export default {
  components: {
    View_restaurant,
  },
  data() {
    return {
      user: null,
      loading: true,
      isAuthenticated: false
    };
  },
  methods: {
    logout() {
      localStorage.removeItem('authToken');
      this.$router.push({ path: './pages_connexion_inscription/pages_connexion/connexion' });
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

          // Récupérer les informations spécifiques aux rôles
          const roleResponse = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRoleUser/${userId}`);
          const roles = roleResponse.data.result[0];

          // Ajouter les informations "Roles" à l'utilisateur
          this.user.roles = roles;

          // Vérifier si le rôle est restaurateur et récupérer le restaurant correspondant
          if (roles.restorant === 1) {
            try {
              const restaurantResponse = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRestorantByRestorerId/${userId}`);
              const restaurant = restaurantResponse.data.result;
              if (restaurant) {
                this.$router.push({ path: './restaurateur' });
              } else {
                // Le restaurateur n'a pas de restaurant, rediriger vers la page d'inscription du restaurant
                this.$router.push({ path: './pages_inscription/register_restaurant' });
              }
            }
            catch (error) {
              this.$router.push({ path: './pages_inscription/register_restaurant' });
            }
          }
          if (roles.customer === 1) {
            this.$router.push({ path: './Accueil' });
          }
          if (roles.delivery_person === 1) {
            this.$router.push({ path: './profil_livreur' });
          }
        } catch (error) {
          console.error(error);
          this.loading = false;
        }

      }
    }
  },
};
</script>
  