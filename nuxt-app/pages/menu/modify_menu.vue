<template>
    <div class="container mx-auto p-4">
      <h1 class="text-xl font-bold mb-4">Modifier le Menu</h1>
      <form @submit.prevent="submitForm">
        <div class="mb-4">
          <label for="menuName" class="block mb-2 text-sm font-medium text-gray-900">Nom du Menu:</label>
          <input v-model="menuName" type="text" id="menuName" required class="form-input">
        </div>
        <div class="grid grid-cols-4 gap-4">
          <div v-for="category in categories" :key="category.name" class="text-center">
            <label :for="category.name" class="block mb-2 text-sm font-medium text-gray-900">{{category.label}}:</label>
            <select v-model="selectedProducts[category.name]" :id="category.name" required multiple class="form-select">
              <option v-for="product in filterByCategory(category.name)" :value="product._id" :key="product._id">{{ product.product_name }}</option>
            </select>
          </div>
        </div>
        <button type="submit" class="px-4 py-2 mt-4 text-white bg-blue-500 rounded hover:bg-blue-600">
          Enregistrer les modifications
        </button>
      </form>
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
        try {
          const response = await axios.put(`http://localhost:3000/updateMenu/${this.menuData._id}`, {
            "restorant" : this.restaurant,
            "menu_name" : this.menuName,
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
          const restaurantResponse = await axios.get(`http://localhost:3000/getRestorantByRestorerId/${restaurateurId}`);
          this.restaurant = restaurantResponse.data.result._id;
          const productFromRestaurant = await axios.get(`http://localhost:3000/getAllProductsFromRestorant/${this.restaurant}`);
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
  