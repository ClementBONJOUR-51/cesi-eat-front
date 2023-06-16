<template>
    <div class="bg-emerald-400 ">
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
          <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0">
              <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                  <form @submit.prevent="submitForm('restaurateur')" v-if="this.$route.query.profil === 'restaurateur'" class="space-y-4 md:space-y-6" action="#">
                    <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl">
                      Créé un compte restaurateur
                    </h1>
                    <div class="grid grid-cols-2 gap-2">
                      <div>
                          <label for="nom" class="block mb-2 text-sm font-medium text-gray-900">Nom</label>
                          <input id="nom" v-model="restaurateurForm.nom" name="nom" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Nom" required="">
                      </div>  
                      <div>
                          <label for="prenom" class="block mb-2 text-sm font-medium text-gray-900">Prenom</label>
                          <input id="prenom" v-model="restaurateurForm.prenom" name="prenom" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Prenom" required="">
                      </div>
                    </div>
    
                    <div class="grid grid-cols-2 gap-2">
                      <div>
                          <label for="code_postal" class="block mb-2 text-sm font-medium text-gray-900">Code Postal</label>
                          <input id="code_postal" v-model="restaurateurForm.code_postal" name="code_postal" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Code Postal" required="">
                      </div>  
                      <div>
                          <label for="rue_num" class="block mb-2 text-sm font-medium text-gray-900">Numéro de rue</label>
                          <input id="rue_num" v-model="restaurateurForm.rue_num" name="rue_num" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Numéro de rue" required="">
                      </div>
                    </div>
    
                      <div>
                          <label for="adresse" class="block mb-2 text-sm font-medium text-gray-900">Adresse</label>
                          <input id="adresse" v-model="restaurateurForm.adresse" name="adresse" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Adresse" required="">
                      </div>
                      <div>
                          <label for="naissance" class="block mb-2 text-sm font-medium text-gray-900">Date de naissance</label>
                          <input id="naissance" v-model="restaurateurForm.naissance" type="date" name="naissance" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Date de naissance" required="">
                      </div>
    
                      <input type="submit" class="w-full text-white bg-yellow-400 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
                  </form>
                  
              </div>
          </div>
      </div>
    </div>
    
    </template>
      
      <script>
      export default {
        name: 'inscription',
        data() {
          return {
            clientForm: {
              nom: '',
              prenom: '',
              code_postal: '',
              rue_num: '',
              adresse: '',
              naissance: ''
            },
            restaurateurForm: {
              nom: '',
              prenom: '',
              code_postal: '',
              rue_num: '',
              adresse: '',
              naissance: ''
            },
            livreurForm: {
              nom: '',
              prenom: '',
              code_postal: '',
              rue_num: '',
              adresse: '',
              naissance: '',
              moyen_deplacement: ''
            }
          }
        },
        methods: {
      onSubmit() {
        this.submitForm('restaurateur');
        this.$emit('formSubmitted'); // When the form is submitted, emit an 'formSubmitted' event
      },
      submitForm(userType) {
        let userData;
        if(userType === 'client') {
          userData = this.clientForm;
        } else if(userType === 'restaurateur') {
          userData = this.restaurateurForm;
        } else if(userType === 'livreur') {
          userData = this.livreurForm;
        }
  
        // You are emitting an event with the user data as payload
        this.$emit('formSubmitted', {userType, userData});
        this.$router.push({ name: 'inscription_after' });
      }
  
        }
      }
      </script>