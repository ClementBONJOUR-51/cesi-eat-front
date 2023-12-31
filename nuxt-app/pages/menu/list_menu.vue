<template>
  <div v-if="isAuthenticated" class="container mx-auto p-4">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-4">
      <div class="mb-8">
        <div class="flex items-center justify-between">
          <button @click="goToProfile" class="px-4 py-2 rounded bg-red-500 flex items-center mb-4">
            <img class="h-6 w-6 mr-2" src="../../src/images/burger-king.jpg" alt="Retour au profil">
            <span>Retour au profil</span>
          </button>
          <button @click="goToAddMenu" class="px-4 py-2 rounded bg-cyan-600 text-white mb-4">
            Ajouter un menu
          </button>
        </div>
        <div class="flex mb-4">
          <input v-model="searchTerm" class="flex-grow px-4 py-2 border rounded shadow-md mr-2" type="search"
            placeholder="Rechercher un menu...">
          <select v-model="filterCategory" class="px-4 py-2 border rounded shadow-md">
            <option value="">Tous</option>
            <option v-for="category in uniqueCategories" :value="category">{{ category }}</option>
          </select>
        </div>
      </div>
      <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
        <div :class="getMenuCardClass(index)" v-for="(menu, index) in filteredMenus" :key="menu._id">
          <h2 class="font-bold mb-8">{{ menu.menu_name }}</h2>
          <h3 class="font-bold mb-2">Entrées</h3>
          <p v-for="product in menu.menu_starters">{{ product.product_name }}</p>
          <h3 class="font-bold mb-2">Plats</h3>
          <p v-for="product in menu.menu_dishes">{{ product.product_name }}</p>
          <h3 class="font-bold mb-2">Boissons</h3>
          <p v-for="product in menu.menu_beverages">{{ product.product_name }}</p>
          <h3 class="font-bold mb-2">Desserts</h3>
          <p v-for="product in menu.menu_desserts">{{ product.product_name }}</p>
          <p>{{ menu.menu_category }}</p>
          <div class="flex justify-end mt-4">
            <button @click="editMenu(menu)" class="px-4 py-2 rounded bg-emerald-500 text-white mr-2">
              Modifier
            </button>
            <button @click="confirmDeleteMenu(menu)" class="px-4 py-2 rounded bg-red-500 text-white">
              Supprimer
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Boîte de dialogue modale de confirmation de suppression -->
    <div v-if="showDeleteConfirmation" class="fixed inset-0 flex items-center justify-center z-50 bg-black bg-opacity-75">
      <div class="bg-white p-4 rounded shadow-md">
        <h2 class="text-lg font-bold mb-4">Êtes-vous sûr de vouloir supprimer ce menu ?</h2>
        <div class="flex justify-end">
          <button @click="deleteMenu(menuToDelete)" class="px-4 py-2 bg-red-500 text-white rounded mr-2">
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
      menus: [],
      searchTerm: '',
      filterCategory: '',
      isAuthenticated: false,
      showDeleteConfirmation: false,
      menuToDelete: null,
      colorClasses: ['bg-cyan-700', 'bg-cyan-600', 'bg-cyan-500'],
    };
  },
  computed: {
    filteredMenus() {
      let filtered = this.menus;

      if (this.searchTerm !== '') {
        filtered = filtered.filter(menu =>
          menu.menu_name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }

      if (this.filterCategory !== '') {
        filtered = filtered.filter(menu =>
          menu.menu_category === this.filterCategory
        );
      }

      return filtered;
    },
    uniqueCategories() {
      return [...new Set(this.menus.map(menu => menu.menu_category))];
    },
  },
  methods: {
    getMenuCardClass(index) {
      const colorClass = this.colorClasses[index % this.colorClasses.length];
      return `menu-card rounded p-4 ${colorClass}`;
    },
    logout() {
      localStorage.removeItem('authToken');
      this.$router.push({ path: '../pages_connexion/connexion' });
    },
    goToProfile() {
      this.$router.push({ path: '../../restaurateur' });
    },
    goToAddMenu() {
      this.$router.push({ path: './add_menu' });
    },
    editMenu(menu) {
      // Stocker les données du menu dans le stockage local
      localStorage.setItem('menuToModify', JSON.stringify(menu));
      // Rediriger vers la page de modification du menu
      this.$router.push({ path: `./modify_menu` });
    },
    confirmDeleteMenu(menu) {
      this.menuToDelete = menu;
      this.showDeleteConfirmation = true;
    },
    async deleteMenu(menu) {
      try {
        await axios.delete(`${useRuntimeConfig().public.api_base_url}/deleteMenu/${menu._id}`);
        // Mettre à jour la liste des menus
        this.menus = this.menus.filter(m => m._id !== menu._id);
      } catch (error) {
        console.error(error);
      }
      this.cancelDelete();
    },
    cancelDelete() {
      this.showDeleteConfirmation = false;
      this.menuToDelete = null;
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
          localStorage.setItem('restaurantId', restoId);

          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getAllMenusByRestorant/${restoId}`);
          this.menus = response.data.result.menus;
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
.menu-card {
  box-shadow: 0 0.5em 1em -0.125em rgb(10 10 10 / 10%), 0 0 0 1px rgb(10 10 10 / 2%);
}
</style>
