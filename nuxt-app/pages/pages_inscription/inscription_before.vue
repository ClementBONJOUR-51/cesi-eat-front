<template>
    <div class="bg-emerald-400 ">
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen sm:h-screen lg:py-0">
          <div class="w-full bg-white rounded-lg shadow dark:border xl:p-0 xl:max-w-2xl md:mt-0 sm:max-w-md ">
              <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                  <h1 class="text-xl font-bold text-center leading-tight tracking-tight text-gray-900 md:text-2xl">
                      Choisisez votre style de profil
                  </h1>
                  <form class="space-y-4 md:space-y-6 " action="#">
                    <div class="grid grid-cols-3">
                        <div class="text-center">
                            <NuxtLink :to="{ path: 'inscription', query: { profil: 'client' , formStep: 1}}"><button class="text-black uppercase bg-emerald-400 rounded-lg shadow-lg shadow-white-500/50 px-8 py-2 my-4 mx-4 transtion duration-300
                            hover:scale-110">Client</button></NuxtLink>
                        </div>
                        <div class="text-center">
                            <NuxtLink :to="{ path: 'inscription', query: { profil: 'restaurateur' , formStep: 1}}"><button class="text-black uppercase bg-emerald-400 rounded-lg shadow-lg shadow-white-500/50 px-8 py-2 my-4 mx-4 transtion duration-300
                            hover:scale-110">Restaurateur</button></NuxtLink>
                        </div>
                        <div class="text-center">
                            <NuxtLink :to="{ path: 'inscription', query: { profil: 'livreur' , formStep: 1}}"><button class="text-black uppercase bg-emerald-400 rounded-lg shadow-lg shadow-white-500/50 px-8 py-2 my-4 mx-4 transtion duration-300
                            hover:scale-110">Livreur</button></NuxtLink>
                        </div>
                    </div>
                    <div class="text-right text-sm">
                        <NuxtLink to="../connexion"><button class="text-black text-xs uppercase bg-emerald-400 rounded-lg shadow-lg shadow-white-500/50 px-4 py-1 my-2 mx-2 transtion duration-300
                        hover:scale-110">Retour</button></NuxtLink>
                    </div>
                  </form>
              </div>
          </div>
      </div>
    </div>
    
    </template>
      
    <script>
      export default {
        name: 'connexion',
      }
    </script>