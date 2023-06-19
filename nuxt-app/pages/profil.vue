<template>
    <div v-if="isAuthenticated">
      <h1>Bienvenue, {{ user.firstname }}</h1>
      <p>Votre adresse e-mail est : {{ user.email }}</p>
      <!-- Ajoutez d'autres détails de l'utilisateur ici. -->
      <button @click="logout">Se déconnecter</button>
    </div>
    <div v-else>
      <p>Veuillez vous connecter pour accéder à cette page.</p>
      <NuxtLink to="../pages_connexion/connexion">Se connecter</NuxtLink>
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
    logout() {
      // Supprimer le token de l'espace de stockage local
      localStorage.removeItem('authToken');
      // Rediriger l'utilisateur vers la page de connexion
      this.$router.push({ path: './pages_connexion/connexion' });
    }
  },
  async created() {
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        console.log(decoded);
        const userId = decoded.id;
        console.log(userId);
        try {
          const response = await axios.get(`http://localhost:3000/getUser/${userId}`);
          this.user = response.data.result;
          console.log(response);
          this.loading = false;
          this.isAuthenticated = true;
        } catch (error) {
          console.error(error);
          this.loading = false;
          // Gérer les erreurs ici.
        }
      }
    }
  },
};
  </script>
