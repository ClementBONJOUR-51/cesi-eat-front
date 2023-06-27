<template>
  <div class="bg-blue-500">
    <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border xl:p-0 xl:max-w-2xl md:mt-0 sm:max-w-md">
        <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
          <h1 class="text-xl font-bold text-center leading-tight tracking-tight text-gray-900 md:text-2xl">
            Ajouter un menu
          </h1>
          <form class="space-y-4 md:space-y-6" @submit.prevent="submitForm">
            <div class="mb-4">
              <label for="menuName" class="block mb-2 text-sm font-medium text-gray-900">Nom du Menu:</label>
              <input v-model="menuName" type="text" id="menuName" required 
                class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5"/>
            </div>
            <div class="grid grid-cols-4 gap-4">
              <div v-for="category in categories" :key="category.name" class="text-center">
                <label :for="category.name" class="block mb-2 text-sm font-medium text-gray-900">{{category.label}}:</label>
                <select v-model="selectedProducts[category.name]" :id="category.name" required multiple
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
                  <option v-for="product in filterByCategory(category.name)" :value="product._id" :key="product._id">{{ product.product_name }}</option>
                </select>
              </div>
            </div>
            <button type="submit"
              class="w-full text-white bg-blue-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
              Ajouter
            </button>
          </form>
          <NuxtLink to="./list_menu"
            class="text-black text-xs uppercase bg-blue-500 rounded-lg shadow-lg shadow-white-500/50 px-4 py-1 my-2 mx-2 transtion duration-300 hover:scale-110">
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
    filterByCategory(category) {
      return this.products.filter(product => product.product_category === this.categories.find(c => c.name === category).label);
    },
    async submitForm() {
      // Send product data to server
      try {
        const response = await axios.post('http://localhost:3000/createMenu', {
          "restorant" : this.restaurant,
          "menu_name" : this.menuName,
          // Send the product IDs as an array of objects
          "menu_starters": this.selectedProducts['menu_starters'].map(id => ({ id_product: id })),
          "menu_dishes": this.selectedProducts['menu_dishes'].map(id => ({ id_product: id })),
          "menu_beverages": this.selectedProducts['menu_beverages'].map(id => ({ id_product: id })),
          "menu_desserts": this.selectedProducts['menu_desserts'].map(id => ({ id_product: id })),
        });
        console.log(response.data);
        // Redirect user to another page
        this.$router.push({ path: "./list_menu" });
      } catch (error) {
        console.error(error);
        // Handle errors here.
      }
    },
  },
  async created() {
    // Assume you get the restaurateur ID this way.
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        const restaurateurId = decoded.id;
        const restaurantResponse = await axios.get(`http://localhost:3000/getRestorantByRestorerId/${restaurateurId}`);
        this.restaurant = restaurantResponse.data.result._id;
        const productFromRestaurant = await axios.get(`http://localhost:3000/getAllProductsFromRestorant/${this.restaurant}`);
        this.products = productFromRestaurant.data.result.products;
      }
    }
  }
}
</script>