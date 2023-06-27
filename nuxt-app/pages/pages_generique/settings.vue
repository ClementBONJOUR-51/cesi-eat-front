<template>
    <div class="bg-emerald-400 h-screen flex flex-col justify-end">
        <div class="flex items-left justify-center h-[25%] flex-col">
            <div class="m-5">
                <composants_generiquePrevButton/>
            </div>
            <span class="text-2xl m-5">Bonjour {{ user.name }},</span>
        </div>
        <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px]">
            <nuxt-link to="/pages_generique/Historique" class="w-[100%] inline-flex items-center justify-center p-5 text-base font-medium text-gray-500 rounded-lg bg-white hover:text-emerald-400 hover:bg-gray-100">
                <span class="w-full">Mes Commandes</span>
                <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
            </nuxt-link>
            <nuxt-link to="/pages_generique/profil" class="w-[100%] inline-flex items-center justify-center p-5 text-base font-medium text-gray-500 rounded-lg bg-white hover:text-emerald-400 hover:bg-gray-100">
                <span class="w-full">Mes Informations</span>
                <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
            </nuxt-link>
            <nuxt-link to="" class="w-[100%] inline-flex items-center justify-center p-5 text-base font-medium text-gray-500 rounded-lg bg-white hover:text-emerald-400 hover:bg-gray-100">
                <span class="w-full">Mes Rôles</span>
                <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
            </nuxt-link>
            <nuxt-link to="" class="w-[100%] inline-flex items-center justify-center p-5 text-base font-medium text-gray-500 rounded-lg bg-white hover:text-emerald-400 hover:bg-gray-100">
                <span class="w-full">F.A.Q.</span>
                <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
            </nuxt-link>
            <nuxt-link to="" class="w-[100%] inline-flex items-center justify-center p-5 text-base font-medium text-gray-500 rounded-lg bg-white hover:text-emerald-400 hover:bg-gray-100">
                <span class="w-full">Notifications</span>
                <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
            </nuxt-link>
            <div  class="w-[100%] inline-flex items-center justify-center p-5 text-base font-medium text-gray-500 rounded-lg bg-white hover:text-emerald-400 hover:bg-gray-100">
                <div class="w-[50%] h-[10%] inline-flex items-center justify-center p-5 text-base font-medium text-white rounded-lg bg-red-500">
                    <button  @click="logout">Se déconnecter</button>
                </div>
            </div>
            
            
        </div>
        
        <composants_generiqueMobileNavBar class="md:hidden"/>
    </div>
</template>

<script>
export default {
    name: 'Accueil',
    data() {
        return {
            user : {
                name: 'Yann',
                email: 'yann.raelle@viacesi.fr'
            },
            
        }
    },
    methods: {
        logout() {
        // Supprimer le token de l'espace de stockage local
        localStorage.removeItem('authToken');
        // Rediriger l'utilisateur vers la page de connexion
        this.$router.push({ path: '../pages_connexion/connexion' });
        },
        checkAuthentication() {
            
            const token = localStorage.getItem('authToken');
            console.log(token);
            if (token) {
                
            } else {
                this.redirectToLoginPage();
            }
        },
        redirectToLoginPage() {
            this.$router.push({ path: '../pages_connexion/connexion' });
        }
    },
    mounted() {
        this.checkAuthentication();
    }
}

</script>
