<template>
    <div>
        <composants_generiqueNavBar class="sticky"/>
        <composants_generiqueSearchBar class="md:hidden"/>
        <div class="flex gap-4 m-5 items-center justify-center">
            <components_customersCard-category v-bind:key="categorie" v-for="categorie in categories" :categorie="categorie"/>
        </div>
        <components_customersMain-Page-explorer/>
        <composants_generiqueMobileNavBar class="md:hidden"/>
    </div>
</template>

<script>
import axios from 'axios';
import { useRouter } from 'vue-router';

import jwt_decode from "jwt-decode";

export default {
    name: 'Acceuil',
    data() {
        return {
            categories:{
                Promo: {
                    Img:'../src/images/burger-king.jpg',
                    Nom: 'Promo',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
                Fastfood: {
                    Img:'../src/images/trois-brasseurs.jpg',
                    Nom: 'Fastfood',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
                Healthy: {
                    Img:'../src/images/trois-brasseurs.jpg',
                    Nom: 'Healthy',
                    Note: '4.5',
                    Nombre_de_note: '100',
                },
            }
        }
    },
    methods: {
        checkAuthentication() {
            
            const token = localStorage.getItem('authToken');
            console.log(token);
            if (token) {
                
            } else {
                this.redirectToLoginPage();
            }
        },
        redirectToLoginPage() {
            this.$router.push({ path: './pages_connexion/connexion' });
        }
    },
    mounted() {
        this.checkAuthentication();
    }
}

</script>

