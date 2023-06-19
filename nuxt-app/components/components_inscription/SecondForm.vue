<template>
    <div class="bg-yellow-400 ">
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
          <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0">
              <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                  <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl">
                      Créé toi un compte
                  </h1>
                  <form class="space-y-4 md:space-y-6" action="#" @submit.prevent="submitForm">
                      <div>
                          <label for="email" class="block mb-2 text-sm font-medium text-gray-900">Email</label>
                          <input id="email" v-model="email" type="email" name="email" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5" placeholder="nom@email.com" required="">
                      </div>
                      <div>
                          <label for="password" class="block mb-2 text-sm font-medium text-gray-900">Mot de passe</label>
                          <input id="password" v-model="password" type="password" name="password" placeholder="••••••••" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 " required="">
                      </div>
                      <div>
                          <label for="passwordConfirm" class="block mb-2 text-sm font-medium text-gray-900">Confimer votre mot de passe</label>
                          <input id="passwordConfirm" v-model="passwordConfirm" type="password" name="password" placeholder="••••••••" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 " required="">
                      </div>
                      <button type="button" @click.prevent="submitForm" class="w-full text-white bg-yellow-400 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">Continuer</button>
                  </form>
              </div>
          </div>
      </div>
    </div>
    
    </template>
    
    
    <script>
    import axios from 'axios';
    export default {
      name: 'inscription',
      props: {
    userData: {
      type: Object,
    },
  },
      data() {
        return {
          email: "",
          password: "",
          passwordConfirm: ""
        };
      },      
      methods: {
       async submitForm() {
       if (this.password !== this.passwordConfirm) {
      alert("Les mots de passe ne correspondent pas !");
      return;
    }
    
    console.log("ici",this.userData);


    const authData = {
    "firstname" : this.userData.firstname,
    "lastname" : this.userData.lastname,
    "gender" : this.userData.gender,
    "birthday" : this.userData.birthday,
    "postal_code" : this.userData.postal_code,
    "street" : this.userData.street,
    "city" : this.userData.city,
    "street_number" : this.userData.street_number,
    "customer": this.userData.customer,
    "email" : this.email,
    "password" : this.password,
    "token" : "ddd",
    "phone" : this.userData.phone,
    "email_sponsor" : this.userData.email_sponsor,
    "lati" : -1.1,
    "longi" : -2.3,
    "delivery_person": this.userData.delivery_person,
    "restorant": this.userData.restorant,
    "administrator": 0,
    "sales_department": 0,
    "technical_department": 0,
    "developer_tier": 0
    };
    console.log(authData);

    try {
      const response = await axios.post('http://localhost:3000/createUser', authData);
      console.log(response.data);
    } catch (error) {
      console.error(error);

    }
  }
}
};
</script>