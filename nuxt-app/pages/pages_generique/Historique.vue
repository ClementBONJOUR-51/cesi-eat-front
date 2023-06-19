<template>
    <div class="bg-emerald-400 h-screen flex flex-col justify-end">
        <div class="flex items-left justify-center h-[25%] flex-col">
            <div class="m-5">
                <composants_generiquePrevButton/>
            </div>
            <span class="text-2xl m-5">Mes Commandes</span>
        </div>
        <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px]">
            <div class="gap-1 flex flex-col items-center justify-center">
                <components_customersCard-history v-bind:key="commande" v-for="commande in commandes" :commande="commande"/>
            </div>
            
        </div>
        <composants_generiqueMobileNavBar class="md:hidden"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            commandes : {
                commande1: {
                    Nom: 'McDonalds',
                    Note: '4.5',
                    Nombre_de_note: '100',
                    Img: '../src/images/bigmac.jpg',
                    plat: {
                        NomPlat: 'BigMac',
                        Prix: '5.50'
                    },
                    date: '12/12/2021',
                    heure: '12:00',
                    prix: '5.50'
                },
                commande2: {
                    Nom: 'Burger King',
                    Note: '4.3',
                    Nombre_de_note: '251',
                    Img: '../src/images/whopper.jpg',
                    plat: {
                        NomPlat: 'Whopper',
                        Prix: '7.50'
                    },
                    date: '12/12/2021',
                    heure: '12:00',
                    prix: '5.50'
                },
                commande3: {
                    Nom: 'KFC',
                    Note: '3.9',
                    Nombre_de_note: '56',
                    Img: '../src/images/tenders.jpg',
                    plat: {
                        NomPlat: 'Tenders',
                        Prix: '4.50'
                    },
                    date: '12/12/2021',
                    heure: '12:00',
                    prix: '5.50'
                },
                
            }
        }
    }
}
</script>