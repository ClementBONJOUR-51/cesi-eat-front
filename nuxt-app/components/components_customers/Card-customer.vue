<template>
    <div class="bg-yellow-400 max-w-xs rounded-[15px] shadow-lg">
        <img class="rounded-[15px] h-[80%]" :src="restaurant.Img">
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-l md:text-xl mb-2">{{ restaurant.Nom }}</div>
            <div class="font-bold text-l md:text-xl mb-2">{{ restaurant.Note }}({{ restaurant.Nombre_de_note }}+)</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        restaurant: {
            type: Object,
            required: true
        }
    }
}
</script>

