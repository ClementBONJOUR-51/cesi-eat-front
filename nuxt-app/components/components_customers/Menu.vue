<template>
    <div class="bg-emerald-400 max-w-xs rounded-[15px] shadow-lg mb-4 mr-4">
        <div class="px-0 py-0 grid grid-cols-1 gap-2">
            <div class="col-span-full">
                <img class="rounded-[15px] w-full h-32 object-cover" :src="'https://picsum.photos/300/200'" />
            </div>
            <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                <div class="flex items-center">
                    Menu
                </div>
            </div>
            <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                <div>
                    <small>prix €</small>
                </div>
            </div>
            <div class="w-full px-4">
                <button @click="$emit('add-to-cart', product)"
                    class="bg-white hover:bg-emerald-600 hover:text-white font-bold mb-2 py-2 px-4 rounded-full w-full">
                    Voir
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['menu'],
    methods: {
        addToCart(menu) {
            this.$emit('add-to-cart', menu);
        },
    },
    mounted() {
        console.log(this.menu);
    },
};
</script>
