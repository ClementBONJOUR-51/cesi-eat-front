<template>
    <div class="bg-yellow-400 max-w-xs rounded-[20px] shadow-lg">
        <img class="rounded-[20px] h-[80%]" :src="restaurant.Img">
        <div class="px-6 py-2 flex justify-between">
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

