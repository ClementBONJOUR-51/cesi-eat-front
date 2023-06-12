<template>
    <div>
        <composants_generiqueNavBar/>
        <div id="categorie" class="bg-blue-500">
            ok
        </div>
        <div class="bg-grey-500 grid grid-cols-8">
            <div id="filtre" class="bg-red-500 col-span-2"> ok </div>
            <div id="recherche" class=" col-span-6">
                <div class="grid grid-flow-row auto-rows-max">
                    <div class="p-8 flex item-center justify-between" v-bind:key="restaurant" v-for="restaurant in restaurants"> 
                        <composants_clientCard :restaurant="restaurant"/>
                    </div>
                    <div>02</div>
                    <div>03</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Acceuil',
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
            }
        }
    },
    components: {
    }
}

</script>