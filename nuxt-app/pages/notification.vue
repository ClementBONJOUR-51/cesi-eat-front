<template>
  <div>
    <!-- Autres contenus de la page -->

    <NotificationPage ref="notificationPage"/>
  </div>
</template>

<script>
import NotificationPage from '~/components/component_notification/notificationPage.vue';

export default {
  components: {
    NotificationPage
  },
  mounted() {
    this.$nextTick(() => {
      this.$refs.notificationPage.addNotification({
        id: 3,
        type: 'success',
        header: 'Success',
        message: 'Operation completed successfully.'
      });

      this.$refs.notificationPage.addNotification({
        id: 4,
        type: 'error',
        header: 'Error',
        message: "Vous n'avez pas les droits pour effectuer cette opération."
      });
    });
  }
};
</script>