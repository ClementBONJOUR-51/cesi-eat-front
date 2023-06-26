<template>
    <div class="bg-emerald-400 max-w-xs rounded-[15px] shadow-lg">
      <router-link :to="'/restaurant/' + restaurant._id">
        <div class="px-0 py-0 grid grid-cols-1 gap-2">
          <div class="col-span-full">
            <img class="rounded-[15px] w-full" :src="restaurant.Img" />
          </div>
          <div class="col-span-full font-bold text-l md:text-xl mb-2 px-4 flex justify-between">
            <div class="flex items-center">
              {{ restaurant.restorant_name }}
            </div>
            <div>
              <!-- {{ restaurant.Note }} ({{ restaurant.Nombre_de_note }}+) -->
            </div>
          </div>
        </div>
      </router-link>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'Card',
    props: {
      restaurant: {
        type: Object,
        required: true,
      },
    },
  };
  </script>
  