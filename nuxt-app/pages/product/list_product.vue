<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-4">
      <div class="mb-8">
        <div class="flex items-center justify-between">
          <button @click="goToProfile" class="px-4 py-2 rounded bg-red-500 flex items-center mb-4">
            <img class="h-6 w-6 mr-2" src="../../src/images/burger-king.jpg" alt="Retour au profil">
            <span>Retour au profil</span>
          </button>
          <button @click="goToAddProduct" class="px-4 py-2 rounded bg-yellow-500 text-white mb-4">
            Ajouter un article
          </button>
        </div>
        <div class="flex mb-4">
          <input
            v-model="searchTerm"
            class="flex-grow px-4 py-2 border rounded shadow-md mr-2"
            type="search"
            placeholder="Rechercher un article..."
          >
          <select v-model="filterCategory" class="px-4 ml-2 mr-4 py-2 border rounded shadow-md">
            <option value="">Tous</option>
            <option v-for="category in uniqueCategories" :value="category">{{ category }}</option>
          </select>
        </div>
      </div>
      <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
        <div :class="getProductCardClass(index)" v-for="(product, index) in filteredProducts" :key="product.id">
          <h2 class="font-bold mb-2">{{ product.product_name }}</h2>
          <p class="mb-2">{{ product.product_price }} €</p>
          <p>{{ product.product_category }}</p>
          <div class="flex justify-end mt-4">
            <button @click="editProduct(product)" class="px-4 py-2 rounded bg-emerald-500 text-white mr-2">
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
  <div v-else class="container mx-auto p-4">
    <p>Veuillez vous connecter pour accéder à cette page.</p>
    <button @click="logout" class="py-2 px-4 bg-blue-500 text-white rounded">Se connecter</button>
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
      colorClasses: ['bg-yellow-500', 'bg-yellow-400', 'bg-yellow-300'],
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
    getProductCardClass(index) {
      const colorClass = this.colorClasses[index % this.colorClasses.length];
      return `product-card rounded p-4 ${colorClass}`;
    },
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
      localStorage.setItem('productToModify', JSON.stringify(product));
      const restoId = product.restoId;
      this.$router.push({ path: './modify_product' });
    },
    confirmDeleteProduct(product) {
      this.productToDelete = product;
      this.showDeleteConfirmation = true;
    },
    async deleteProduct(product) {
      try {
        await axios.delete(`http://localhost:3000/deleteProduct/${product._id}`);
        this.products = this.products.filter(p => p.id !== product.id);
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
          const check = await axios.get(`http://localhost:3000/getUser/${userId}`);
          this.user = check.data.result;
          this.loading = false;
          this.isAuthenticated = true;

          const resto = await axios.get(`http://localhost:3000/getRestorantByRestorerId/${userId}`);
          const restoId = resto.data.result._id;
          localStorage.setItem('restaurantId', restoId);

          const response = await axios.get(`http://localhost:3000/getAllProductsFromRestorant/${restoId}`);
          this.products = response.data.result.products;
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