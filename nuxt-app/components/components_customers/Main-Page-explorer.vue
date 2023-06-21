<template>
  <div class="bg-grey-500">
    <div class="grid grid-flow-row auto-rows-max">
      <div class="px-10 pt-1 font-bold text-2xl md:text-3xl mb-2">
        Resto
      </div>
      <div
        class="custom-scrollbar p-8 md:p-5 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto"
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

export default {
  setup() {
    const restaurantsfusion = ref([]);
    const restaurants = ref([]);

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
      restaurantsfusion.value = newVal.map((restaurant) => ({
        ...restaurant,
        Img: 'https://picsum.photos/500/300',
        Note : Math.floor(Math.random() * 5) + 1,
        Nombre_de_note : Math.floor(Math.random() * 100) + 1,
      }));

      console.log(restaurantsfusion.value, 'test');
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