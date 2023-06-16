<template>
    <div class="bg-emerald-400 ">
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
          email: "",
          password: "",
          passwordConfirm: ""
        };
      },
      methods: {
  submitForm() {
    if (this.password !== this.passwordConfirm) {
      alert("Les mots de passe ne correspondent pas !");
      return;
    }

    this.$store.commit('setUserData', {
      ...this.$store.state.userData,
      email: this.email,
      password: this.password // Note : ne stockez jamais les mots de passe en clair dans un vrai scénario !
    });

    this.$emit('formSubmitted');
  }
}
    };
    </script>