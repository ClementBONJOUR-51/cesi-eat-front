<template>
    <div v-if="isAuthenticated" class="container mx-auto p-4">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-4">
            <div class="flex items-center justify-between mb-8">
                <button @click="goToProfile" class="px-4 py-2 rounded bg-gray-200 flex items-center">
                    <img class="h-6 w-6 mr-2" src="../../src/images/burger-king.jpg" alt="Retour au profil">
                    <span>Retour au profil</span>
                </button>
                <input 
                    v-model="searchTerm"
                    class="px-4 py-2 border rounded shadow-md ml-4 mr-2 flex-grow" 
                    type="search" 
                    placeholder="Rechercher un menu..."
                >
                <select v-model="filterCategory" class="px-4 ml-2 mr-4 py-2 border rounded shadow-md">
                    <option value="">Tous</option>
                    <option v-for="category in uniqueCategories" :value="category">{{ category }}</option>
                </select>
                <button @click="goToAddMenu" class="px-4 py-2 rounded bg-green-500 text-white">
                    Ajouter un menu
                </button>
            </div>
            <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
                <div class="menu-card bg-yellow-300 rounded p-4" v-for="menu in filteredMenus" :key="menu._id">
                    <h2 class="font-bold mb-2">{{ menu.menu_name }}</h2>
                    <p class="mb-2">{{ menu.menu_price }} €</p>
                    <p>{{ menu.menu_category }}</p>
                    <div class="flex justify-end mt-4">
                        <button @click="editMenu(menu)" class="px-4 py-2 rounded bg-green-500 text-white mr-2">
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
      menus: [],
      searchTerm: '',
      filterCategory: '',
      isAuthenticated: false,
      showDeleteConfirmation: false,
      menuToDelete: null,
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
        await axios.delete(`http://localhost:3000/deleteMenu/${menu._id}`);
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
          const check = await axios.get(`http://localhost:3000/getUser/${userId}`);
          this.user = check.data.result;
          this.loading = false;
          this.isAuthenticated = true;

          const resto = await axios.get(`http://localhost:3000/getRestorantByRestorerId/${userId}`);
          const restoId = resto.data.result._id;
          localStorage.setItem('restaurantId', restoId);

          const response = await axios.get(`http://localhost:3000/getAllMenusByRestorant/${restoId}`);
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
