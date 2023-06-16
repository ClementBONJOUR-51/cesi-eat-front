<template>
    <div class="bg-yellow-400 w-4/12 md:w-2/12 rounded-[15px] shadow-lg">
        <img class="rounded-[15px] h-[80%] w-[100%]" :src="categorie.Img">
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-sm md:text-xl mb-2 items-center justify-center">{{ categorie.Nom }}</div>
        </div>
    </div>
</template> 

<script>
export default {
    name: 'Card-category',
    props: {
        categorie: {
            type: Object,
            required: true
        }
    }
}
</script>