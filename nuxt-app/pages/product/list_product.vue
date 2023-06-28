<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-4">
      <div class="flex items-center justify-between mb-8">
        <button @click="goToProfile" class="px-4 py-2 rounded bg-gray-200 flex items-center">
          <img class="h-6 w-6 mr-2" src="../../src/images/burger-king.jpg" alt="Retour au profil">
          <span>Retour au profil</span>
        </button>
        <input v-model="searchTerm" class="px-4 py-2 border rounded shadow-md ml-4 mr-2 flex-grow" type="search"
          placeholder="Rechercher un article...">
        <select v-model="filterCategory" class="px-4 ml-2 mr-4 py-2 border rounded shadow-md">
          <option value="">Tous</option>
          <option v-for="category in uniqueCategories" :value="category">{{ category }}</option>
        </select>
        <button @click="goToAddProduct" class="px-4 py-2 rounded bg-green-500 text-white">
          Ajouter un article
        </button>
      </div>
      <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
        <div class="product-card bg-yellow-300 rounded p-4" v-for="product in filteredProducts" :key="product.id">
          <h2 class="font-bold mb-2">{{ product.product_name }}</h2>
          <p class="mb-2">{{ product.product_price }} €</p>
          <p>{{ product.product_category }}</p>
          <div class="flex justify-end mt-4">
            <button @click="editProduct(product)" class="px-4 py-2 rounded bg-green-500 text-white mr-2">
              Modifier
            </button>
            <button @click="confirmDeleteProduct(product)" class="px-4 py-2 rounded bg-red-500 text-white">
              Supprimer
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Boîte de dialogue modale de confirmation de suppression -->
    <div v-if="showDeleteConfirmation" class="fixed inset-0 flex items-center justify-center z-50 bg-black bg-opacity-75">
      <div class="bg-white p-4 rounded shadow-md">
        <h2 class="text-lg font-bold mb-4">Êtes-vous sûr de vouloir supprimer cet article ?</h2>
        <div class="flex justify-end">
          <button @click="deleteProduct(productToDelete)" class="px-4 py-2 bg-red-500 text-white rounded mr-2">
            Oui
          </button>
          <button @click="cancelDelete" class="px-4 py-2 bg-gray-500 text-white rounded">
            Non
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-else class="w-screen h-screen bg-emerald-500 m-0 flex items-center justify-center flex-col">
    <h1 class="text-white m-4 text-2xl">Veuillez vous connecter pour accéder à cette page.</h1>
    <button @click="logout"
      class="bg-transparent hover:bg-white text-white font-semibold hover:text-white py-2 px-4 border border-white hover:border-transparent rounded">
      Se connecter
    </button>
  </div>
</template>
  
<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";

export default {
  data() {
    return {
      products: [],
      searchTerm: '',
      filterCategory: '',
      isAuthenticated: false,
      showDeleteConfirmation: false,
      productToDelete: null,
    };
  },
  computed: {
    filteredProducts() {
      let filtered = this.products;

      if (this.searchTerm !== '') {
        filtered = filtered.filter(product =>
          product.product_name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }

      if (this.filterCategory !== '') {
        filtered = filtered.filter(product =>
          product.product_category === this.filterCategory
        );
      }

      return filtered;
    },
    uniqueCategories() {
      return [...new Set(this.products.map(product => product.product_category))];
    },
  },
  methods: {
    logout() {
      localStorage.removeItem('authToken');
      this.$router.push({ path: '../pages_connexion/connexion' });
    },
    goToProfile() {
      this.$router.push({ path: '../../restaurateur' });
    },
    goToAddProduct() {
      this.$router.push({ path: './add_product' });
    },
    editProduct(product) {
      // Stocker les données du produit dans le stockage local
      localStorage.setItem('productToModify', JSON.stringify(product));
      // Récupérer l'ID du restaurant du produit
      const restoId = product.restoId;
      // Rediriger vers la page de modification du produit
      this.$router.push({ path: './modify_product' });
    },
    confirmDeleteProduct(product) {
      this.productToDelete = product;
      this.showDeleteConfirmation = true;
    },
    async deleteProduct(product) {
      try {
        await axios.delete(`${useRuntimeConfig().public.api_base_url}/deleteProduct/${product._id}`);
        // Mettre à jour la liste des produits
        this.products = this.products.filter(p => p.id !== product.id);
        console.log(this.products);
      } catch (error) {
        console.error(error);
      }
      this.cancelDelete();
    },
    cancelDelete() {
      this.showDeleteConfirmation = false;
      this.productToDelete = null;
    },
  },
  async created() {
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const userId = decoded.id;
        try {
          const check = await axios.get(`${useRuntimeConfig().public.api_base_url}/getUser/${userId}`);
          this.user = check.data.result;
          this.loading = false;
          this.isAuthenticated = true;

          const resto = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRestorantByRestorerId/${userId}`);
          const restoId = resto.data.result._id;
          console.log("test", restoId);
          // Stocker l'ID du restaurant dans le stockage local
          localStorage.setItem('restaurantId', restoId);

          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getAllProductsFromRestorant/${restoId}`);
          this.products = response.data.result.products;
          console.log(this.products);
        } catch (error) {
          console.error(error);
          this.loading = false;
        }
      }
    }
  },
}
</script>
  
<style scoped>
.product-card {
  box-shadow: 0 0.5em 1em -0.125em rgb(10 10 10 / 10%), 0 0 0 1px rgb(10 10 10 / 2%);
}
</style>
  