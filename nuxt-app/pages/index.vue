<template>
    <v-app>
        <div class="bg-emerald-400 h-screen">
            <div class="flex flex-col items-center justify-center mx-auto md:h-screen sm:h-screen lg:py-0">
                <h1 class="text-center font-bold tracking-wide font-mono text-7xl p-4 mt-52">CesiEat</h1>
                <h4 class="text-center p-4">Nourrir l'ingénieur qui est en vous !</h4>
                <div class="text-center">
                    <NuxtLink to="pages_connexion/connexion"><button class="text-black uppercase bg-white rounded-lg shadow-lg shadow-white-500/50 px-8 py-2 my-4 mx-4 mb-72 transtion duration-300
                hover:bg-white hover:shadow-inner  hover:scale-110">Connexion</button></NuxtLink>
                </div>
            </div>
        </div>
    </v-app>
</template>

<script setup>

</script>

<style lang="scss" scoped></style>