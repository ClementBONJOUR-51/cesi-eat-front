<template>
  <div v-if="isAuthenticated" class="bg-emerald-400 h-screen flex flex-col justify-end">
    <div class="flex items-left justify-center h-[25%] flex-col">
      <div class="m-5">
        <composants_generiquePrevButton/>
      </div>
      <span class="text-2xl m-5">Mes Informations</span>
    </div>
    <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px]">
      <div class="mt-4 border-t border-gray-100">
        <dl class="divide-y divide-gray-100">
          <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
            <dt class="text-sm font-medium leading-6 text-gray-900">Prénom</dt>
            <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">
              <template v-if="editing">
                <input v-model="editedUser.firstname" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </template>
              <template v-else>
                {{ user.firstname }}
              </template>
            </dd>
          </div>
          <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
            <dt class="text-sm font-medium leading-6 text-gray-900">Nom</dt>
            <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">
              <template v-if="editing">
                <input v-model="editedUser.lastname" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </template>
              <template v-else>
                {{ user.lastname }}
              </template>
            </dd>
          </div>
          <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
            <dt class="text-sm font-medium leading-6 text-gray-900">Date de naissance</dt>
            <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">
              <template v-if="editing">
                <input v-model="editedUser.birthday" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </template>
              <template v-else>
                {{ user.birthday }}
              </template>
            </dd>
          </div>
          <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
            <dt class="text-sm font-medium leading-6 text-gray-900">Téléphone</dt>
            <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">
              <template v-if="editing">
                <input v-model="editedUser.phone" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </template>
              <template v-else>
                {{ user.phone }}
              </template>
            </dd>
          </div>
          <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
            <dt class="text-sm font-medium leading-6 text-gray-900">Genre</dt>
            <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">
              <template v-if="editing">
                <input v-model="editedUser.gender" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </template>
              <template v-else>
                {{ user.gender }}
              </template>
            </dd>
          </div>
          <div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
            <dt class="text-sm font-medium leading-6 text-gray-900">Email</dt>
            <dd class="mt-1 text-sm leading-6 text-gray-700 sm:col-span-2 sm:mt-0">
              <template v-if="editing">
                <input v-model="editedUser.email" type="text" required
                  class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5">
              </template>
              <template v-else>
                {{ user.email }}
              </template>
            </dd>
          </div>
        </dl>
      </div>
      <div class="flex justify-center space-x-4 mt-6">
        <template v-if="editing">
          <button @click="cancelEdit" class="text-white bg-red-500 hover:bg-red-700 px-4 py-2 rounded-lg">Annuler</button>
          <button @click="applyEdit" class="text-white bg-blue-500 hover:bg-blue-700 px-4 py-2 rounded-lg">Appliquer</button>
        </template>
        <template v-else>
          <button @click="startEdit" class="text-white bg-yellow-500 hover:bg-yellow-700 px-4 py-2 rounded-lg">Modifier</button>
          <button @click="showDeleteConfirmation = true" class="text-white bg-red-500 hover:bg-red-700 px-4 py-2 rounded-lg">Supprimer</button>
        </template>
      </div>
    </div>
    <div v-if="showDeleteConfirmation" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
      <div class="bg-white p-8 rounded-lg">
        <p class="text-lg font-medium mb-4">Vous vous apprêtez à supprimer votre compte, cette action est irréversible.</p>
        <p class="mb-4">Si vous voulez supprimer votre compte, veuillez saisir le code suivant : {{ deleteConfirmationCode }}</p>
        <input v-model="deleteInput" type="text" class="bg-gray-100 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 mb-4">
        <p class="text-lg mb-4">Une fois le code saisi, appuyez sur "Supprimer".</p>
        <div class="flex justify-center space-x-4">
          <button @click="deleteUser" class="text-white bg-red-500 hover:bg-red-700 px-4 py-2 rounded-lg">Supprimer</button>
          <button @click="cancelDelete" class="text-white bg-gray-500 hover:bg-gray-700 px-4 py-2 rounded-lg">Annuler</button>
        </div>
      </div>
    </div>
    <composants_generiqueMobileNavBar class="md:hidden"/>
  </div>
</template>

<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";
import { format } from 'date-fns';


export default {
  data() {
    return {
      user: null,
      editedUser: null,
      loading: true,
      isAuthenticated: false,
      editing: false,
      showDeleteConfirmation: false,
      deleteConfirmationCode: generateRandomCode(),
      deleteInput: ''
    };
  },
  methods: {
    logout() {
      // Supprimer le token de l'espace de stockage local
      localStorage.removeItem('authToken');
      // Rediriger l'utilisateur vers la page de connexion
      this.$router.push({ path: './pages_connexion/connexion' });
    },
    startEdit() {
      // Copier les informations de l'utilisateur dans l'objet d'édition
      this.editedUser = { ...this.user };
      this.editing = true;
    },
    cancelEdit() {
      // Réinitialiser l'objet d'édition et désactiver le mode d'édition
      this.editedUser = null;
      this.editing = false;
    },
    applyEdit() {
      // Envoyer les informations mises à jour au serveur
      const token = localStorage.getItem('authToken');
      const decoded = jwt_decode(token);
      const userId = decoded.id;
      
      const updateUserURL = `http://localhost:3000/updateUser/${userId}`;
      console.log(updateUserURL);
      axios
        .put(updateUserURL, {
          firstname: this.editedUser.firstname,
          lastname: this.editedUser.lastname,
          gender: this.editedUser.gender,
          birthday: this.editedUser.birthday,
          email: this.editedUser.email,
          password: this.editedUser.password,
          token: token,
          phone: this.editedUser.phone,
          email_sponsor: this.editedUser.email_sponsor,
          postal_code: this.editedUser.postal_code,
          street: this.editedUser.street,
          city: this.editedUser.city,
          street_number: this.editedUser.street_number,
          lati: this.editedUser.lati,
          longi: this.editedUser.longi,
          customer: this.editedUser.customer,
          delivery_person: this.editedUser.delivery_person,
          restorant: this.editedUser.restorant,
          administrator: this.editedUser.administrator,
          sales_department: this.editedUser.sales_department,
          technical_department: this.editedUser.technical_department,
          developer_tier: this.editedUser.developer_tier
        })
        .then(response => {
          console.log(response.data);
          // Mettre à jour les informations de l'utilisateur avec les nouvelles données
          this.user = { ...this.editedUser };
          // Réinitialiser l'objet d'édition et désactiver le mode d'édition
          this.editedUser = null;
          this.editing = false;
        })
        .catch(error => {
          console.error(error);
          // Gérer les erreurs ici.
        });
    },
    deleteUser() {
      if (this.deleteInput === this.deleteConfirmationCode) {
        const token = localStorage.getItem('authToken');
        const decoded = jwt_decode(token);
        const userId = decoded.id;
        
        const deleteUserURL = `http://localhost:3000/deleteUser/${userId}`;
        console.log(deleteUserURL);
        axios
          .delete(deleteUserURL)
          .then(response => {
            console.log(response.data);
            // Supprimer le token de l'espace de stockage local
            localStorage.removeItem('authToken');
            // Rediriger l'utilisateur vers la page de connexion
            this.$router.push({ path: '../pages_connexion/connexion' });
          })
          .catch(error => {
            console.error(error);
            // Gérer les erreurs ici.
          });
      }
    },
    cancelDelete() {
      this.showDeleteConfirmation = false;
      this.deleteInput = '';
    }
  },
  computed: {
    formattedBirthday() {
      if (this.user && this.user.birthday) {
        const date = new Date(this.user.birthday);
        const year = date.getFullYear();
        const month = ('0' + (date.getMonth() + 1)).slice(-2);
        const day = ('0' + date.getDate()).slice(-2);
        return `${year}-${month}-${day}`;
      }
      return '';
    }
  },
  async created() {
    if (process.client) {
      const token = localStorage.getItem('authToken');
      if (token) {
        const decoded = jwt_decode(token);
        console.log(decoded);
        const userId = decoded.id;
        console.log(userId);
        try {
          const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getUser/${userId}`);
          this.user = response.data.result;
          this.user.birthday = format(new Date(this.user.birthday), 'yyyy-MM-dd');
          console.log(response);
          this.loading = false;
          this.isAuthenticated = true;
        } catch (error) {
          console.error(error);
          // Gérer les erreurs ici.
        }
      }
    }
  }
};

function generateRandomCode() {
  const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
  let code = '';
  for (let i = 0; i < 5; i++) {
    const randomIndex = Math.floor(Math.random() * characters.length);
    code += characters[randomIndex];
  }
  return code;
}
</script>
