<template>
  <div class="bg-cyan-600 min-h-screen flex flex-col items-center justify-center">
    <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border xl:p-0 xl:max-w-2xl md:mt-0 sm:max-w-md">
        <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
          <h1 class="text-xl font-bold text-center leading-tight tracking-tight text-gray-900 md:text-2xl">
            Modifier le Menu
          </h1>
          <form class="space-y-4 md:space-y-6" @submit.prevent="submitForm">
            <div class="mb-4">
              <label for="menuName" class="block mb-2 text-sm font-medium text-gray-900">Nom du Menu:</label>
              <input v-model="menuName" type="text" id="menuName" required 
                class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-cyan-600 focus:border-cyan-600 block w-full p-2.5"/>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
              <div v-for="category in categories" :key="category.name" class="text-center">
                <label :for="category.name" class="block mb-2 text-sm font-medium text-gray-900">{{category.label}}:</label>
                <select v-model="selectedProducts[category.name]" :id="category.name" required multiple
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-cyan-500 focus:border-cyan-500 block w-full p-2.5">
                  <option v-for="product in filterByCategory(category.name)" :value="product._id" :key="product._id">{{ product.product_name }}</option>
                </select>
              </div>
            </div>
            <button type="submit"
              class="w-full text-white bg-cyan-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-cyan-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
              Enregistrer les modifications
            </button>
            <button
              @click="goTolistMenu"
              class="w-full text-white bg-cyan-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-cyan-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
            Retour
          </button>
          </form>
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
      menuName: '',
      products: [],
      categories: [
        { name: 'menu_starters', label: 'Entrée' },
        { name: 'menu_dishes', label: 'Plat' },
        { name: 'menu_beverages', label: 'Boisson' },
        { name: 'menu_desserts', label: 'Dessert' },
      ],
      selectedProducts: {
        'menu_starters': [],
        'menu_dishes': [],
        'menu_beverages': [],
        'menu_desserts': []
      },
      restaurant: '' // Suppose restaurant ID is stored here.
    }
  },
  methods: {
<<<<<<< HEAD
=======
    goTolistMenu() {
      this.$router.push({ path: './list_menu' });
    },
>>>>>>> origin/card-components
    filterByCategory(category) {
      return this.products.filter(product => product.product_category === this.categories.find(c => c.name === category).label);
    },
    async submitForm() {
      try {
<<<<<<< HEAD
        const response = await axios.put(`${useRuntimeConfig().public.api_base_url}/updateMenu/${this.menuData._id}`, {
          "restorant": this.restaurant,
          "menu_name": this.menuName,
=======
        const response = await axios.put(`http://localhost:3000/updateMenu/${this.menuData._id}`, {
          "restorant" : this.restaurant,
          "menu_name" : this.menuName,
>>>>>>> origin/card-components
          "menu_starters": this.selectedProducts['menu_starters'].map(id => ({ id_product: id })),
          "menu_dishes": this.selectedProducts['menu_dishes'].map(id => ({ id_product: id })),
          "menu_beverages": this.selectedProducts['menu_beverages'].map(id => ({ id_product: id })),
          "menu_desserts": this.selectedProducts['menu_desserts'].map(id => ({ id_product: id })),
        });
        console.log(response.data);
        this.$router.push({ path: "./list_menu" });
      } catch (error) {
        console.error(error);
      }
    },
  },
  async created() {
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const restaurateurId = decoded.id;
<<<<<<< HEAD
        const restaurantResponse = await axios.get(`${useRuntimeConfig().public.api_base_url}/getRestorantByRestorerId/${restaurateurId}`);
        this.restaurant = restaurantResponse.data.result._id;
        const productFromRestaurant = await axios.get(`${useRuntimeConfig().public.api_base_url}/getAllProductsFromRestorant/${this.restaurant}`);
=======
        const restaurantResponse = await axios.get(`http://localhost:3000/getRestorantByRestorerId/${restaurateurId}`);
        this.restaurant = restaurantResponse.data.result._id;
        const productFromRestaurant = await axios.get(`http://localhost:3000/getAllProductsFromRestorant/${this.restaurant}`);
>>>>>>> origin/card-components
        this.products = productFromRestaurant.data.result.products;
      }

      const menuToModify = localStorage.getItem('menuToModify');
      if (menuToModify) {
        this.menuData = JSON.parse(menuToModify);
        this.menuName = this.menuData.menu_name;

        for (const category in this.selectedProducts) {
          this.selectedProducts[category] = this.menuData[category].map(product => product.id_product);
        }
      }
    }
  }
}
</script>
<<<<<<< HEAD
  
=======
>>>>>>> origin/card-components
