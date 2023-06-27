<template>
    <div v-if="isAuthenticated" class="bg-emerald-400 h-screen flex flex-col justify-end">
        <div class="flex items-left justify-center h-[25%] flex-col">
          <div class="m-5">
                <composants_generiquePrevButton/>
            </div>
            <span class="text-2xl m-5">Mes Informations</span>
        </div>
        <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px]">
          <div class="mt-4 border-t border-gray-100">
            <dl class="divide-y divide-gray-100">
              <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
                <dt class="text-sm font-medium leading-6 text-gray-900">Prénom</dt>
                <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">{{ user.firstname }}</dd>
              </div>
              <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
                <dt class="text-sm font-medium leading-6 text-gray-900">Nom</dt>
                <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">{{ user.lastname }}</dd>
              </div>
              <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
                <dt class="text-sm font-medium leading-6 text-gray-900">Date de naissance</dt>
                <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">{{ user.birthday }}</dd>
              </div>
              <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
                <dt class="text-sm font-medium leading-6 text-gray-900">Téléphone</dt>
                <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">{{ user.phone }}</dd>
              </div>
              <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
                <dt class="text-sm font-medium leading-6 text-gray-900">Genre</dt>
                <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">{{ user.email }}</dd>
              </div>
              <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
                <dt class="text-sm font-medium leading-6 text-gray-900">Email</dt>
                <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">{{ user.lastname }}</dd>
              </div>
            </dl>
          </div>
        </div>
        <composants_generiqueMobileNavBar class="md:hidden"/>
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
        const userId = decoded.id;
        try {
          const response = await axios.get(`http://localhost:3000/getUser/${userId}`);
          this.user = response.data.result;
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
