<template>
  <div>
    <!-- Autres contenus de la page -->

    <NotificationPage ref="notificationPage"/>
    <button @click="executeMountedLogic">Exécuter la logique de mounted()</button>
  </div>
</template>

<script>
import NotificationPage from '~/components/component_notification/notificationPage.vue';

export default {
  components: {
    NotificationPage
  },
  methods: {
    // changer le message de la notification
    messageSuccess() {
      return 'Votre message a été envoyé avec succès';
    },

    messageError() {
      return 'Une erreur est survenue lors de l\'envoi de votre message';
    },

  executeMountedLogic() {
      this.$nextTick(() => {
        this.$refs.notificationPage.addNotification({
          id: 3,
          type: 'success',
          header: 'Success',
          message: this.messageSuccess(),
        });

        this.$refs.notificationPage.addNotification({
          id: 4,
          type: 'error',
          header: 'Error',
          message: this.messageError(),
        });
      });
    }
  }
};
</script>
