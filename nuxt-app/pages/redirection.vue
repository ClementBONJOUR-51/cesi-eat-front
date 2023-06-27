<template>
    <div v-if="isAuthenticated" class="container mx-auto p-4">
      <navbarNavbar />
      <div class="mb-4">
        <h1 class="text-xl font-bold">Bienvenue, {{ user.firstname }}</h1>
        <p class="text-gray-700">Votre adresse e-mail est : {{ user.email }}</p>
      </div>
      <div class="">
        <div v-if="isAuthenticated">
        <template v-if="user.roles && user.roles.restorant === 1">
          <View_restaurant /> 
        </template>
        <template v-else-if="user.roles && user.roles.customer === 1">
          <view-client /> 
        </template>
        <template v-else="user.roles && user.roles.delivery_person  === 1">
          <view-livreur />
        </template>
      </div>
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
  </template>
  
    
    <script>
  import axios from 'axios';
  import jwt_decode from "jwt-decode";
  import View_restaurant from '../components/components_view/View_restaurant';
  export default {
    components:{
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
        this.$router.push({ path: './pages_connexion/connexion' });
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
            console.log("ici", this.user);
            this.loading = false;
            this.isAuthenticated = true;
  
            // Récupérer les informations spécifiques aux rôles
            const roleResponse = await axios.get(`http://localhost:3000/getRoleUser/${userId}`);
            const roles = roleResponse.data.result[0];
            console.log("a", roles.restorant);
  
            // Ajouter les informations "Roles" à l'utilisateur
            this.user.roles = roles;
            
          // Vérifier si le rôle est restaurateur et récupérer le restaurant correspondant
          if (roles.restorant === 1) {
            try{
              const restaurantResponse = await axios.get(`http://localhost:3000/getRestorantByRestorerId/${userId}`);
              const restaurant = restaurantResponse.data.result;
              if (restaurant) {
                this.$router.push({ path: './restaurateur' });
                console.log("Le restaurateur possède un restaurant :", restaurant);
              } else {
                // Le restaurateur n'a pas de restaurant, rediriger vers la page d'inscription du restaurant
                console.log("Le restaurateur n'a pas de restaurant");
                this.$router.push({ path: './pages_inscription/register_restaurant' });
              }
            }
            catch(error){
              this.$router.push({ path: './pages_inscription/register_restaurant' });
            }
            }
            if (roles.customer === 1) {
                this.$router.push({ path: './Accueil' });
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
  