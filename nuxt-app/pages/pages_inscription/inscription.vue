<template>
        <components_inscriptionClientForm v-if="userType === 'client' && formStep === 1" @formSubmitted="handleFormSubmitted"/>
        <components_inscriptionSecondForm v-if="userType === 'client' && formStep === 2" />
        <components_inscriptionRestaurateurForm v-if="userType === 'restaurateur' && formStep === 1" @formSubmitted="handleFormSubmitted"/>
        <components_inscriptionSecondForm v-if="userType === 'restaurateur' && formStep === 2" />
        <components_inscriptionLivreurForm v-if="userType === 'livreur' && formStep === 1" @formSubmitted="handleFormSubmitted"/>
        <components_inscriptionSecondForm v-if="userType === 'livreur' && formStep === 2" />
</template>
  
<script>
export default {
    data() {
        return {
            userType: this.$route.query.profil,
            formStep: Number(this.$route.query.formStep)
        }
    },
    methods: {
        
        handleFormSubmitted(formData) {
    // Vous avez maintenant accès aux données du formulaire dans formData
    this.formStep = 2;
    this.$router.push({ query: { ...this.$route.query, formStep: '2' } });
        }
    },
    watch: {
        '$route'(to, from) {
            this.userType = to.query.profil;
            this.formStep = Number(to.query.formStep);
        }
    }
}
</script>