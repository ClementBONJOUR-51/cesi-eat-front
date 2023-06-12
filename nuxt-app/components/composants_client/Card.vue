<template>
    <div class="bg-yellow-400 max-w-sm rounded-[20px] overflow-hidden shadow-lg">
        <img class="w-full rounded-[20px]" :src="restaurant.Img" alt="Sunset in the mountains">
        <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{ restaurant.Nom }}</div>
            <div class="font-bold text-xl mb-2">{{ restaurant.Note }}({{ restaurant.Nombre_de_note }}+)</div>
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

