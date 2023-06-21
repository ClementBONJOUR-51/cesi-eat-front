<template>
    <div class="bg-grey-500">
        <div class="grid grid-flow-row auto-rows-max">
            <div class="px-10 pt-1 font-bold text-2xl md:text-3xl mb-2">
                Commander à nouveau
            </div>
            <div class="custom-scrollbar p-8 md:p-5 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto" >
                <components_customersCard-customer v-bind:key="restaurant" v-for="restaurant in this.restaurantsfusion" :restaurant="restaurant" to="/index"/>
            </div>
            <div class="px-10 pt-1 font-bold text-2xl md:text-3xl mb-2">
                Explorer
            </div>
            <div class="custom-scrollbar p-8 md:p-5 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto" >
                <components_customersCard-customer v-bind:key="restaurant" v-for="restaurant in this.restaurantsfusion" :restaurant="restaurant"/>
            </div>
            <div class="px-10 pt-1 font-bold text-2xl md:text-3xl mb-2">
                Produits populaires
            </div>
            <div class="custom-scrollbar p-8 md:p-5 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto" >
                <components_customersCard-customer  v-bind:key="restaurant" v-for="restaurant in this.restaurantsfusion" :restaurant="restaurant"/>
            </div>
            <div class="md:hidden py-2 gap-1 flex flex-col items-center justify-center">
                <div :key="restaurant.id" v-for="restaurant in restaurantsfusion">
                    <NuxtLink to="../../pages_generique/Restaurant/${restaurant.id}" >
                        <components_customersCard-customer class="my-2"  :restaurant="restaurant"/>
                    </NuxtLink>
                </div>
                
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { ref, onMounted, watch } from 'vue';

export default {
  setup() {
    const restaurantsfusion = ref([]);
    const restaurants = ref([]);
    const datadur = [
      {
        id: 1,
        Img: '../src/images/burger-king.jpg',
        Nom: 'Burger King',
        Note: '4.5',
        Nombre_de_note: '100',
      },
      {
        id: 2,
        Img: '../src/images/trois-brasseurs.jpg',
        Nom: '3 Brasseurs',
        Note: '4.5',
        Nombre_de_note: '100',
      },
      {
        id: 3,
        Img: '../src/images/trois-brasseurs.jpg',
        Nom: '4 Brasseurs',
        Note: '4.5',
        Nombre_de_note: '100',
      },
    ];

    onMounted(async () => {
      try {
        const response = await axios.get('http://localhost:3000/getAllRestorants');
        restaurants.value = response.data.result.restorants;
        console.log(restaurants.value);
      } catch (error) {
        console.error(error);
      }
    });

    watch(restaurants, (newVal) => {
      // Vérification de la longueur des deux listes
      console.log(newVal.length);
      console.log(datadur.length);
      if (newVal.length !== datadur.length) {
        console.error('Les listes ne sont pas de même longueur');
        return;
      }

      // Fusion des objets
      const restaurantsfusionValue = newVal.map((restaurant, index) => {
        const datadurObjet = datadur[index];

        // Création d'un nouvel objet fusionné
        return {
          ...restaurant, // Copie tous les champs de l'objet "restaurants"
          ...datadurObjet, // Copie tous les champs de l'objet "datadur"
        };
      });

      restaurantsfusion.value = restaurantsfusionValue; // Assigner le résultat à la propriété restaurantsfusion

      console.log(restaurantsfusion.value, "test");
    });

    return {
      restaurantsfusion,
      restaurants,
      datadur,
    };
  },
};
</script>

<style>
.custom-scrollbar::-webkit-scrollbar {
  width: 8px;
  height: 8px;
  border-radius: 10px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  background-color: #ffffff;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: #d4d3d3;
}

.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background-color: #bbbbbb;
}
</style>

