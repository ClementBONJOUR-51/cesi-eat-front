<template>
    <div>
      <button @click="toggleNotification">Afficher la notification</button>
      <Notification
        v-if="show"
        type="error"
        header="Error"
        message="Ceci est une notification de succès"
        @show="handleNotificationShow"
        @closed="handleNotificationClosed"
        :duration="5000"
        :closable="true"
      />
    </div>
  </template>
  
  <script>
  import Notification from '~/components/component_notification/notification_simple.vue';
  
  export default {
    components: {
      Notification,
    },
    data() {
      return {
        show: false,
      };
    },
    methods: {
      toggleNotification() {
        this.show = true;
      },
      handleNotificationShow() {
        // Handle show event if needed
      },
      handleNotificationClosed() {
        this.show = false;
      },
    },
  };
  </script>
  