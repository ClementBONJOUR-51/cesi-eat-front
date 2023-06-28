<template>
  <div class="container mx-auto p-4">
    <h1 class="text-xl font-bold text-emerald-700 mb-4">Modifier le profil</h1>
    <form @submit.prevent="submitForm">
      <div class="grid grid-cols-2 gap-4">
        <div>
          <label for="firstname" class="block mb-2 text-sm font-medium text-gray-900">Prénom :</label>
          <input v-model="profileData.firstname" id="firstname" type="text" required class="form-input">
        </div>
        <div>
          <label for="lastname" class="block mb-2 text-sm font-medium text-gray-900">Nom :</label>
          <input v-model="profileData.lastname" id="lastname" type="text" required class="form-input">
        </div>
        <div>
          <label for="gender" class="block mb-2 text-sm font-medium text-gray-900">Genre :</label>
          <select v-model="profileData.gender" id="gender" class="form-select">
            <option value="male">Homme</option>
            <option value="female">Femme</option>
            <option value="other">Autre</option>
          </select>
        </div>
        <div>
          <label for="birthday" class="block mb-2 text-sm font-medium text-gray-900">Date de naissance :</label>
          <input v-model="profileData.birthday" id="birthday" type="date" class="form-input">
        </div>
        <div>
          <label for="email" class="block mb-2 text-sm font-medium text-gray-900">E-mail :</label>
          <input v-model="profileData.email" id="email" type="email" required class="form-input">
        </div>
        <div>
          <label for="password" class="block mb-2 text-sm font-medium text-gray-900">Mot de passe :</label>
          <input v-model="profileData.password" id="password" type="password" class="form-input">
        </div>
        <div>
          <label for="phone" class="block mb-2 text-sm font-medium text-gray-900">Téléphone :</label>
          <input v-model="profileData.phone" id="phone" type="text" class="form-input">
        </div>
        <div>
          <label for="postal_code" class="block mb-2 text-sm font-medium text-gray-900">Code postal :</label>
          <input v-model="profileData.postal_code" id="postal_code" type="text" class="form-input">
        </div>
        <div>
          <label for="street" class="block mb-2 text-sm font-medium text-gray-900">Rue :</label>
          <input v-model="profileData.street" id="street" type="text" class="form-input">
        </div>
        <div>
          <label for="city" class="block mb-2 text-sm font-medium text-gray-900">Ville :</label>
          <input v-model="profileData.city" id="city" type="text" class="form-input">
        </div>
        <div>
          <label for="street_number" class="block mb-2 text-sm font-medium text-gray-900">Numéro de rue :</label>
          <input v-model="profileData.street_number" id="street_number" type="number" class="form-input">
        </div>
        <div>
          <label for="lati" class="block mb-2 text-sm font-medium text-gray-900">Latitude :</label>
          <input v-model="profileData.lati" id="lati" type="number" step="0.000001" class="form-input">
        </div>
        <div>
          <label for="longi" class="block mb-2 text-sm font-medium text-gray-900">Longitude :</label>
          <input v-model="profileData.longi" id="longi" type="number" step="0.000001" class="form-input">
        </div>
      </div>
      <div class="grid md:grid-cols-1 gap-4">
      <button type="submit" class="px-4 py-2 mt-4 text-white bg-emerald-700 rounded hover:bg-emerald-800">
        Enregistrer les modifications
      </button>
      <button @click="goToRestaurateur" class="px-4 py-2 mt-2 text-white bg-emerald-700 rounded hover:bg-emerald-800">
        Retour
      </button>
    </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";

export default {
  data() {
    return {
      profileData: {}, // Données du profil à modifier
    };
  },
  methods: {
    async submitForm() {
      try {
        const token = localStorage.getItem('authToken');
        if (token) {
          const decoded = jwt_decode(token);
          const userId = decoded.id;
          const response = await axios.put(`${useRuntimeConfig().public.api_base_url}/updateUser/${userId}`, this.profileData);
          console.log(response.data);
          // Rediriger l'utilisateur vers la page de profil
          this.$router.push({ path: '../../../restaurateur' });
        }
      } catch (error) {
        console.error(error);
        // Gérer les erreurs ici.
      }
    },
    async fetchProfileData() {
      try {
        const token = localStorage.getItem('authToken');
        if (token) {
          const decoded = jwt_decode(token);
          const userId = decoded.id;
          console.log(decoded);
          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getUser/${userId}`);
          const profileData = response.data.result;
          this.profileData = {
            firstname: profileData.firstname,
            lastname: profileData.lastname,
            gender: profileData.gender,
            birthday: profileData.birthday,
            email: profileData.email,
            password: profileData.password,
            phone: profileData.phone,
            postal_code: profileData.postal_code,
            street: profileData.street,
            city: profileData.city,
            street_number: profileData.street_number,
            lati: profileData.lati,
            longi: profileData.longi,
            customer: decoded.is_customer, // récupérer le rôle depuis le token
            delivery_person: decoded.is_delivery_person, // récupérer le rôle depuis le token
            restorant: decoded.is_restorant, // récupérer le rôle depuis le token
            administrator: 0,
            sales_department: 0,
            technical_department: 0,
            developer_tier: 0,
          };
          console.log(decoded.birthday)
          console.log(profileData)
          console.log(profileData.firstname)
          console.log(decoded.is_customer)
          console.log(decoded.is_delivery_person)
          console.log(decoded.is_restorant)
        }
      } catch (error) {
        console.error(error);
      }
    },
    goToRestaurateur() {
      this.$router.push({ path: '../../restaurateur' });
    },
  },
  created() {
    this.fetchProfileData();
  },
};
</script>
  
