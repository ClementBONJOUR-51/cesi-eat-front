<template>
    <div class="bg-emerald-400 h-screen">
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
          <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0">
              <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                  <form v-if="this.$route.query.profil === 'client'" @submit.prevent="onSubmit" class="space-y-4 md:space-y-6" action="#">
                    <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl">
                      Créé un compte client
                    </h1>
                    <div class="grid grid-cols-2 gap-2">
                      <div>
                          <label for="firstname" class="block mb-2 text-sm font-medium text-gray-900">Prénom</label>
                          <input id="firstname" v-model="clientForm.firstname" name="firstname" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Prénom" required="">
                      </div>
                      <div>
                          <label for="lastname" class="block mb-2 text-sm font-medium text-gray-900">Nom de famille</label>
                          <input id="lastname" v-model="clientForm.lastname" name="lastname" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Nom de famille" required="">
                      </div>
                    </div>
    
                    <div>
                          <label for="phone" class="block mb-2 text-sm font-medium text-gray-900">Téléphone</label>
                          <input id="phone" v-model="clientForm.phone" type="tel" name="phone" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="téléphone" required="">
                      </div>

                    <div>
                          <label for="birthday" class="block mb-2 text-sm font-medium text-gray-900">Date de naissance</label>
                          <input id="birthday" v-model="clientForm.birthday" type="date" name="birthday" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Date de naissance" required="">
                      </div>
                    <div class="grid grid-cols-2 gap-2">
                      <div>
                          <label for="postal_code" class="block mb-2 text-sm font-medium text-gray-900">Code Postal</label>
                          <input id="postal_code" v-model="clientForm.postal_code" name="postal_code" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Code Postal" required="">
                      </div>  
                      <div>
                          <label for="street_number" class="block mb-2 text-sm font-medium text-gray-900">Numéro de rue</label>
                          <input id="street_number" v-model="clientForm.street_number" name="street_number" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Numéro de rue" required="">
                      </div>
                    </div>
    
                    <div class="grid grid-cols-2 gap-2">
                      <div>
                          <label for="street" class="block mb-2 text-sm font-medium text-gray-900">Adresse</label>
                          <input id="street" v-model="clientForm.street" name="street" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Adresse" required="">
                      </div>
                      <div>
                          <label for="city" class="block mb-2 text-sm font-medium text-gray-900">Ville</label>
                          <input id="city" v-model="clientForm.city" name="city" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="Ville" required="">
                      </div>
                    </div> 

                    <div class="grid grid-cols-2 gap-2">
                    <div>
                          <label for="gender" class="block mb-2 text-sm font-medium text-gray-900">Sexe</label>
                          <select v-model="clientForm.gender" class="border py-2 px-4 rounded-lg border-gray-300">
                            <option value="Male">Homme</option>
                            <option value="Female">Femme</option>
                        </select>
                      </div>
                      <div>
                          <label for="email_sponso" class="block mb-2 text-sm font-medium text-gray-900">Parrainage</label>
                          <input id="email_sponso" v-model="clientForm.email_sponsor" type="email" name="email_sponso" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="nom_sponso@email.com">
                      </div>
                      </div>
    
                      <input type="submit" class="w-full text-white bg-emerald-400 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">
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
              firstname : '',
              lastname : '',
              phone : "",
              birthday : "",
              gender : "",
              postal_code : "",
              street_number : '',
              street : "",
              city : "",
              email_sponsor : "",
              customer: 1,
              delivery_person: 0,
              restorant: 0,
            },
            restaurateurForm: {
              firstname : '',
              lastname : '',
              phone : "",
              birthday : "",
              gender : "",
              postal_code : "",
              street_number : '',
              street : "",
              city : "",
              email_sponsor : "",
              customer: 0,
              delivery_person: 0,
              restorant: 1,
            },
            livreurForm: {
              firstname : '',
              lastname : '',
              phone : "",
              birthday : "",
              gender : "",
              postal_code : "",
              street_number : '',
              street : "",
              city : "",
              email_sponsor : "",
              customer: 0,
              delivery_person: 1,
              restorant: 0,
            }
          }
        },
      methods: {
      onSubmit() {
        this.submitForm(this.$route.query.profil);
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
      }
  
        }
      }
      </script>