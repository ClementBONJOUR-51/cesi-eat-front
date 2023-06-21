<template>
    <composants_generiqueNavBar class="sticky"/>
    <div>
        {{ restaurant.restorant_name }}
    </div>
    <div>

    </div>
    <composants_generiqueMobileNavBar class="md:hidden"/>
</template>

<script>
 export default {
    name: 'Restaurant',
    props: {
        restaurant: {
            type: Object,
            required: true
        }
    }
}
 

</script>