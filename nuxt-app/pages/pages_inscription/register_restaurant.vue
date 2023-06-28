<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">Création d'un restaurant</h1>
    <form @submit.prevent="submitForm" class="max-w-md mx-auto">
      <div class="mb-4">
        <label for="restorant-name" class="block font-bold mb-2">Nom du restaurant:</label>
        <input v-model="restorantName" id="restorant-name" type="text" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="mb-4">
        <label for="restorant-type" class="block font-bold mb-2">Type de restaurant:</label>
        <input v-model="restorantType" id="restorant-type" type="text" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="mb-4">
        <label for="phone-number" class="block font-bold mb-2">Numéro de téléphone:</label>
        <input v-model="phoneNumber" id="phone-number" type="tel" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="mb-4">
        <label for="street-number" class="block font-bold mb-2">Numéro de rue:</label>
        <input v-model="address.street_number" id="street-number" type="number" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="mb-4">
        <label for="street" class="block font-bold mb-2">Rue:</label>
        <input v-model="address.street" id="street" type="text" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="mb-4">
        <label for="postal-code" class="block font-bold mb-2">Code postal:</label>
        <input v-model="address.postal_code" id="postal-code" type="text" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="mb-4">
        <label for="city" class="block font-bold mb-2">Ville:</label>
        <input v-model="address.city" id="city" type="text" required
          class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div class="mb-4">
          <label for="latitude" class="block font-bold mb-2">Latitude:</label>
          <input v-model="address.lati" id="latitude" type="number" step="0.000001" required
            class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
        </div>

        <div class="mb-4">
          <label for="longitude" class="block font-bold mb-2">Longitude:</label>
          <input v-model="address.longi" id="longitude" type="number" step="0.000001" required
            class="w-full px-3 py-2 border rounded shadow-md focus:outline-none focus:ring-emerald-400 focus:border-emerald-400">
        </div>
      </div>

      <button type="submit"
        class="w-full mt-8 py-2 px-4 bg-emerald-400 text-white font-bold rounded hover:bg-emerald-500 focus:outline-none focus:ring-emerald-400 focus:ring-2">
        Créer le restaurant
      </button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import jwt_decode from 'jwt-decode';

export default {
  data() {
    return {
      restorantName: '',
      restorantType: '',
      phoneNumber: '',
      restorer: {
        id_user: '',
        lastname: '',
        firstname: '',
        email: '',
        phone: '',
      },
      address: {
        street: '',
        postal_code: '',
        city: '',
        lati: '',
        longi: '',
        street_number: '',
      },
    };
  },
  methods: {
    async submitForm() {
      try {
        const token = localStorage.getItem('authToken');
        if (token) {
          const decoded = jwt_decode(token);
          console.log(decoded);
          this.restorer = {
            id_user: decoded.id,
            lastname: decoded.lastname,
            firstname: decoded.firstname,
            email: decoded.email,
            phone: decoded.phone,
          };
        }

        const response = await axios.post('http://localhost:3000/createRestorant', {
          restorant_name: this.restorantName,
          restorant_type: this.restorantType,
          phone_number: this.phoneNumber,
          restorer: this.restorer,
          address: this.address,
        });

        console.log(response.data);
        // Rediriger l'utilisateur vers une autre page
        this.$router.push({ path: '../restaurateur' });
      } catch (error) {
        console.error(error);
        // Gérer les erreurs ici.
      }
    },
  },
};
</script>
