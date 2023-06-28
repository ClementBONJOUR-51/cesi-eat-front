<template>
  <div class="bg-yellow-600 min-h-screen flex flex-col items-center justify-center">
    <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border xl:p-0 xl:max-w-2xl md:mt-0 sm:max-w-md">
        <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
          <h1 class="text-xl font-bold text-center leading-tight tracking-tight text-gray-900 md:text-2xl">
            Modifier le produit
          </h1>
          <form class="space-y-4 md:space-y-6" @submit.prevent="submitForm">
            <div class="grid grid-cols-2 gap-4">
              <div>
                <label for="name" class="block mb-2 text-sm font-medium text-gray-900">Nom du produit:</label>
                <input v-model="productData.product_name" id="name" type="text" required
                class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" />
              </div>
              <div>
                <label for="price" class="block mb-2 text-sm font-medium text-gray-900">Prix du produit:</label>
                <input v-model="productData.product_price" id="price" type="number" step="0.01" required
                class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" />
              </div>
              <div>
                <label for="type" class="block mb-2 text-sm font-medium text-gray-900">Type de produit:</label>
                <select v-model="productData.product_category" id="type" required class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
                  <option value="">-- Choisissez une option --</option>
                  <option value="Entrée">Entrée</option>
                  <option value="Plat">Plat</option>
                  <option value="Boisson">Boisson</option>
                  <option value="Dessert">Dessert</option>
                  <!-- ajoutez d'autres options ici -->
                </select>
              </div>
            </div>
            <button type="submit" class="w-full text-white bg-yellow-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-cyan-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
              Enregistrer les modifications</button>
            <button @click="goTolistProduct"  class="w-full text-white bg-yellow-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-cyan-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
            Retour</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      productData: null, // Données du produit à modifier
    };
  },
  methods: {
    async submitForm() {
      try {
        // Récupérer l'ID du restaurant depuis le stockage local
        const restaurantId = localStorage.getItem('restaurantId');
        const response = await axios.put(`${useRuntimeConfig().public.api_base_url}/updateProduct/${this.productData._id}`, {
          product_name: this.productData.product_name,
          product_price: this.productData.product_price,
          product_category: this.productData.product_category,
          restorant: restaurantId, // Utiliser le nom du champ correctement
        });
        console.log(response.data);
        // Rediriger l'utilisateur vers la liste des produits
        this.$router.push({ path: "./list_product" });
      } catch (error) {
        console.error(error);
        // Gérer les erreurs ici.
      }
    },
    goTolistProduct() {
      this.$router.push({ path: "./list_product" });
    },
  },
  created() {
    // Récupérer les données du produit depuis le stockage local
    const productToModify = localStorage.getItem('productToModify');
    if (productToModify) {
      this.productData = JSON.parse(productToModify);
      console.log(this.productData); // Vérifier les données du produit dans la console
    }
  },
};
</script>

<style scoped>

</style>
