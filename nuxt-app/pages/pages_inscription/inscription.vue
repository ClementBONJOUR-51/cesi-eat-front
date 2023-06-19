<template>
        <components_inscriptionClientForm v-if="userType === 'client' && formStep === 1" @formSubmitted="handleFormSubmitted"/>
        <components_inscriptionSecondForm :user-data="tempData" v-if="userType === 'client' && formStep === 2" />
        <components_inscriptionRestaurateurForm v-if="userType === 'restaurateur' && formStep === 1" @formSubmitted="handleFormSubmitted"/>
        <components_inscriptionSecondForm :user-data="tempData" v-if="userType === 'restaurateur' && formStep === 2" />
        <components_inscriptionLivreurForm v-if="userType === 'livreur' && formStep === 1" @formSubmitted="handleFormSubmitted"/>
        <components_inscriptionSecondForm :user-data="tempData" v-if="userType === 'livreur' && formStep === 2" />
</template>
  
<script>
export default {
  data() {
    return {
      userType: this.$route.query.profil,
      formStep: Number(this.$route.query.formStep),
      tempData: null // Nouveau
    }
  },
  methods: {
    handleFormSubmitted(formData) {
      if (this.formStep === 1) {
        this.tempData = formData.userData;
        this.formStep = 2;
        this.$router.push({ query: { ...this.$route.query, formStep: '2' } });
      } else if (this.formStep === 2) {
        const combinedData = {
          ...this.tempData,
          ...formData
        };
        this.$emit('formSubmitted', combinedData);
        this.tempData = null;
      }
      
    },
  },
}
</script>