<template>
  <div class="container mx-auto p-4">
    <h1 class="text-xl font-bold mb-4">Modifier le produit</h1>
    <form @submit.prevent="submitForm">
      <div class="grid grid-cols-2 gap-4">
        <div>
          <label for="name" class="block mb-2 text-sm font-medium text-gray-900">Nom du produit:</label>
          <input v-model="productData.product_name" id="name" type="text" required class="form-input">
        </div>
        <div>
          <label for="price" class="block mb-2 text-sm font-medium text-gray-900">Prix du produit:</label>
          <input v-model="productData.product_price" id="price" type="number" step="0.01" required class="form-input">
        </div>
        <div>
          <label for="type" class="block mb-2 text-sm font-medium text-gray-900">Type de produit:</label>
          <select v-model="productData.product_category" id="type" required class="form-select">
            <option value="">-- Choisissez une option --</option>
            <option value="Entrée">Entrée</option>
            <option value="Plat">Plat</option>
            <option value="Boisson">Boisson</option>
            <option value="Dessert">Dessert</option>
            <!-- ajoutez d'autres options ici -->
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
        console.log(restaurantId)
        const response = await axios.put(`http://localhost:3000/updateProduct/${this.productData._id}`, {
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
