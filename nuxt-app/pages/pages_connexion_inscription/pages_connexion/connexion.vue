<template>
  <div class="bg-emerald-400 w-screen h-screen">
    <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0">
        <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
          <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl">
            Connecte toi à ton compte
          </h1>
          <form @sumbit.prevent="submitForm" class="space-y-4 md:space-y-6" method="POST">
            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-gray-900">Email</label>
              <input v-model="email" id="email" type="email" name="email"
                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5"
                placeholder="nom@email.com" required="">
            </div>
            <div>
              <label for="password" class="block mb-2 text-sm font-medium text-gray-900">Mot de passe</label>
              <input v-model="password" id="password" type="password" name="password" placeholder="••••••••"
                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 "
                required="">
            </div>
            <div class="flex items-center justify-between">
              <div class="flex items-start">
                <div class="flex items-center h-5">
                  <input id="remember" aria-describedby="remember" type="checkbox"
                    class="text-emerald-400 focus:ring-emerald-500 w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-primary-300 ">
                </div>
                <div class="ml-3 text-sm">
                  <label for="remember" class="text-gray-500">Se souvenir de moi</label>
                </div>
              </div>
              <a href="#" class="text-sm font-medium text-primary-600 hover:underline transition duration-150 
                          hover:text-emerald-400 hover:decoration-emerald-400 hover:decoration-2">Mot de passe
                oublié?</a>
            </div>
            <button type="submit"
              class="w-full text-white bg-emerald-400 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center"
              @click="loginUser">
              Connexion
            </button>
            <p class="text-sm font-light text-gray-500">
              Tu n'as pas de compte? <NuxtLink to="../../pages_inscription/inscription_before" href="#" class="font-medium text-primary-600 hover:underline transition duration-150 
                          hover:text-emerald-400 hover:decoration-emerald-400 hover:decoration-2">Inscris toi!
              </NuxtLink>
            </p>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
      
<script>
import axios from 'axios'
import jwt_decode from "jwt-decode";

export default {
  name: 'connexion',
  data() {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    async loginUser() {
      const loginRequest = {
        email: this.email,
        password: this.password,
        error: "",
      }

      try {
        const response = await axios.post(`${useRuntimeConfig().public.api_base_url}/login`, loginRequest)
        if (response.data.status == "success") {
          // Stockage du jeton dans le localStorage
          localStorage.setItem('authToken', response.data.response)

          // Lire le token depuis le localstorage
          const token = localStorage.getItem('authToken');

          // Décoder le token
          const decoded = jwt_decode(token);
          this.user = decoded.user;

          // Redirection vers la page de profil
          this.$router.push({ path: '../Accueil' })
        }
        else {
          // Gestion des erreurs d'authentification ici
        }
      }
      catch (error) {
        // Gestion des erreurs de requête ici
      }
    },
        checkAuthentication() {
            
            const token = localStorage.getItem('authToken');
            console.log(token);
            if (token) {
                this.redirectToAcceuil();
            } else {
            }
        },
        redirectToAcceuil() {
            this.$router.push({ path: '../../redirection' });
        }
    },
  mounted() {
    this.checkAuthentication();
  }
}
</script>