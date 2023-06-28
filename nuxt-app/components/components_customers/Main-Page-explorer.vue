<template>
  <div class="bg-grey-500">
    <div class="grid grid-flow-row auto-rows-max">
      <div class="px-10 pt-1 font-bold text-2xl md:text-3xl mb-2">
        Restaurants
      </div>
      <div
        class="flex flex-wrap p-2 justify-center gap-5"
      >
        <components_customersCard-customer
          v-bind:key="restaurant"
          v-for="restaurant in this.restaurantsfusion"
          :restaurant="restaurant"
          to="/index"
        />
      </div>

      <!-- <div class="py-2 gap-1 flex flex-col items-center justify-center">
        <div :key="restaurant.id" v-for="restaurant in restaurantsfusion">
          <NuxtLink
            to="../../pages_generique/Restaurant/${restaurant.id}"
          >
            <components_customersCard-customer
              class="my-2"
              :restaurant="restaurant"
            />
          </NuxtLink>
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { ref, onMounted, watch } from 'vue';
import images from '../../assets/images.json';

export default {
  setup() {
    const restaurantsfusion = ref([]);
    const restaurants = ref([]);

    onMounted(async () => {
      try {
        const response = await axios.get(`${useRuntimeConfig().public.api_base_url}/getAllRestorants`);
        restaurants.value = response.data.result.restorants;
      } catch (error) {
        console.error(error);
      }
    });

    watch(restaurants, (newVal) => {
      restaurantsfusion.value = newVal.map((restaurant) => ({
        ...restaurant,
        Img: images[restaurant.restorant_name],
        Note: Math.floor(Math.random() * 5) + 1,
        Nombre_de_note: Math.floor(Math.random() * 100) + 1,
      }));
    });

    return {
      restaurantsfusion,
      restaurants,
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