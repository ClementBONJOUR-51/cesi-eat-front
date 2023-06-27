<template>
  <div class="bg-green-500">
    <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border xl:p-0 xl:max-w-2xl md:mt-0 sm:max-w-md">
        <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
          <h1 class="text-xl font-bold text-center leading-tight tracking-tight text-gray-900 md:text-2xl">
            Ajouter un produit
          </h1>
          <form class="space-y-4 md:space-y-6" @submit.prevent="submitForm">
            <div class="grid grid-cols-3">
              <div class="text-center">
                <label for="name" class="block mb-2 text-sm font-medium text-gray-900">Nom du produit:</label>
                <input v-model="productName" id="name" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </div>
              <div class="text-center">
                <label for="price" class="block mb-2 text-sm font-medium text-gray-900">Prix du produit:</label>
                <input v-model="productPrice" id="price" type="number" step="0.01" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </div>
              <div class="text-center">
                <label for="type" class="block mb-2 text-sm font-medium text-gray-900">Type de produit:</label>
                <select v-model="productCategory" id="type" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
                  <option value="">--Choisir une option--</option>
                  <option value="Entrée">Entrée</option>
                  <option value="Plat">Plat</option>
                  <option value="Boisson">Boisson</option>
                  <option value="Dessert">Dessert</option>
                  <!-- ajoutez d'autres options ici -->
                </select>
              </div>
            </div>

            <button type="submit"
              class="w-full text-white bg-green-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
              Ajouter
            </button>
          </form>
          <NuxtLink to="./list_product"
            class="text-black text-xs uppercase bg-green-500 rounded-lg shadow-lg shadow-white-500/50 px-4 py-1 my-2 mx-2 transtion duration-300 hover:scale-110">
            Retour
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";

export default {
  data() {
    return {
      productName: '',
      productPrice: '',
      productCategory: '',
      restaurant: '' // Supposons que l'ID du restaurant soit stocké ici.
    }
  },
  methods: {
    async submitForm() {
      // Envoyer les données du produit au serveur
      try {
        const response = await axios.post(`${useRuntimeConfig().public.api_base_url}/createProduct`, {
          "restorant": this.restaurant,
          "product_name": this.productName,
          "product_price": this.productPrice,
          "product_category": this.productCategory,
        });
        console.log(response.data);

        // Rediriger l'utilisateur vers une autre page
        this.$router.push({ path: "./list_product" });
      } catch (error) {
        console.error(error);
        // Gérer les erreurs ici.
      }
    }
  },
  async created() {
    // Supposons que vous obteniez l'ID du restaurateur de cette façon.
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const restaurateurId = decoded.id;
        const restaurantResponse = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRestorantByRestorerId/${restaurateurId}`);
        this.restaurant = restaurantResponse.data.result._id;
      }
    }
  }
}
</script>
