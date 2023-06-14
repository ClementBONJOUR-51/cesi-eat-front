<template>
    <div class="bg-grey-500">
        <div class="grid grid-flow-row auto-rows-max">
            <div class="px-10 py-5 font-bold text-3xl mb-2">
                Commander à nouveau
            </div>
            <div class="custom-scrollbar p-8 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto" >
                <components_clientCard-customer v-bind:key="restaurant" v-for="restaurant in restaurants" :restaurant="restaurant"/>
            </div>
            <div class="p-2  font-bold text-3xl mb-2">
                Explorer
            </div>
            <div class="custom-scrollbar p-8 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto" >
                <components_clientCard-customer v-bind:key="restaurant" v-for="restaurant in restaurants" :restaurant="restaurant"/>
            </div>
            <div class="p-2  font-bold text-3xl mb-2">
                Produits populaires
            </div>
            <div class="custom-scrollbar p-8 grid grid-flow-col auto-cols-max gap-4 content-start overflow-x-auto" >
                <components_clientCard-customer v-bind:key="restaurant" v-for="restaurant in restaurants" :restaurant="restaurant"/>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            restaurants:{
                burger_king: {
                    Img:'../src/images/burger-king.jpg',
                    Nom: 'Burger King',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
                trois_brasseurs: {
                    Img:'../src/images/trois-brasseurs.jpg',
                    Nom: '3 Brasseurs',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
                quatre_brasseurs: {
                    Img:'../src/images/trois-brasseurs.jpg',
                    Nom: '4 Brasseurs',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
                cinq_brasseurs: {
                    Img:'../src/images/trois-brasseurs.jpg',
                    Nom: '5 Brasseurs',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
               six_brasseurs: {
                    Img:'../src/images/trois-brasseurs.jpg',
                    Nom: '5 Brasseurs',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
            }
        }
    },
    components: {
    }
}
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
  background-color: #cacaca;
}

.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background-color: #b3b3b3;
}
</style>