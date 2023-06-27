<template>
    <div class="bg-emerald-400 max-w-xs rounded-[15px] shadow-lg mb-4 mr-4">
        <div class="px-0 py-0 grid grid-cols-1 gap-2">
            <div class="col-span-full">
                <img class="rounded-[15px] object-cover"
                    :src="images[product.product_name] || 'https://picsum.photos/400/200'"
                    style="width: 250px; height: 150px;" />


            </div>
            <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                <div class="flex items-center">
                    {{ product.product_name }}
                </div>
            </div>
            <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                <div>
                    <small>{{ product.product_category + " - " + product.product_price }} €</small>
                </div>
            </div>
            <div class="w-full px-4">
                <button @click="$emit('add-to-cart', product)"
                    class="bg-white hover:bg-emerald-600 hover:text-white font-bold mb-2 py-2 px-4 rounded-full w-full">
                    Ajouter
                </button>
            </div>
        </div>
    </div>
</template>

<script>

import images from '../../assets/images'

export default {
    props: ['product'],
    data() {
        return {
            images: images
        }
    },
    methods: {
        addToCart(product) {
            this.$emit('add-to-cart', product);
        },
    },
};
</script>
