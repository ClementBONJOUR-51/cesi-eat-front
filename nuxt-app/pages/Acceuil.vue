<template>
    <div>
        <composants_generiqueNavBar/>
        <div id="categorie" class="bg-blue-500">
            ok
        </div>
        <components_clientMain-Page-explorer/>
        <composants_generiqueMobileNavBar class="md:hidden"/>
    </div>
</template>

<script>
export default {
    name: 'Acceuil',
   
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