<template>
    <div class="bg-emerald-400 max-w-xs rounded-[15px] mt-5 shadow-lg">
        <div id="test" class="bg-emerald-400 rounded-[15px] mt-5 absolute top-0 right-0">
            <span>{{ commande.date }}</span>
        </div>
        <img class="rounded-[15px] h-[80%] w-[100%]" :src="commande.Img">
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-l md:text-xl mb-2">{{ commande.Nom }}</div>
            <div class="font-bold text-l md:text-xl mb-2">{{ commande.Note }}({{ commande.Nombre_de_note }}+)</div>
        </div>
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-l md:text-xl mb-2">{{ commande.plat.NomPlat }}</div>
            <div class="font-bold text-l md:text-xl mb-2">{{ commande.plat.Prix }} €</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        commande: {
            type: Object,
            required: true
        }
    }
}
</script>

