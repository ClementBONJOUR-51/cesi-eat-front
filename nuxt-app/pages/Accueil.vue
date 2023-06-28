<template>
    <div>
        <composants_generiqueNavBar class="sticky"/>
        <composants_generiqueSearchBar class="md:hidden"/>
        <!-- <div class="flex gap-4 m-5 items-center justify-center">
            <components_customersCard-category v-bind:key="categorie" v-for="categorie in categories" :categorie="categorie"/>
        </div> -->
        <components_customersMain-Page-explorer/>
        <composants_generiqueMobileNavBar class="md:hidden"/>
    </div>
</template>

<script>

export default {
    name: 'Accueil',
    data() {
        return {
            
        }
    },
    methods: {
        checkAuthentication() {
            
            const token = localStorage.getItem('authToken');
            if (token) {
                
            } else {
                this.redirectToLoginPage();
            }
        },
        redirectToLoginPage() {
            this.$router.push({ path: '../pages_connexion_inscription/pages_connexion/connexion' });
        }
    },
    mounted() {
        this.checkAuthentication();
    }
}

</script>

