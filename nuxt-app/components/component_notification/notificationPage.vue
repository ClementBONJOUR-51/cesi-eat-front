<template>
  <div>
    <!-- NotificationIcon ajouté les notifications  -->
    <NotificationIcon :unreadCount="unreadCount" :notifications="notifications" @toggle-notifications="toggleNotifications" />

    <!-- Reste du contenu -->

    <Notification v-if="showNotifications" v-for="notification in notifications" :key="notification.id"
      :type="notification.type" :header="notification.header" :message="notification.message"
      @closed="removeNotification(notification.id)" />
  </div>
</template>

<script>
import NotificationIcon from '~/components/component_notification/notification_icon.vue';
import Notification from '~/components/component_notification/notification.vue';

export default {
  props: {
    customMounted: {
      type: Function,
      default: null
    }
  },
  components: {
    NotificationIcon,
    Notification
  },
  data() {
    return {
      unreadCount: 0,
      notifications: [],
      showNotifications: true
    };
  },
  methods: {
    toggleNotifications() {
      // Mettez à jour l'état d'affichage des notifications
      this.showNotifications = !this.showNotifications;
    },
    addNotification(notification) {
      this.notifications.push(notification);
      this.updateUnreadCount();
    },
    updateUnreadCount() {
      this.unreadCount = this.notifications.length;
    },
    removeNotification(id) {
      const index = this.notifications.findIndex(notification => notification.id === id);
      if (index !== -1) {
        this.notifications.splice(index, 1);
        this.updateUnreadCount();
      }
    }
  },
  mounted() {
    if (this.customMounted && typeof this.customMounted === 'function') {
      this.customMounted(); // Appel de la fonction personnalisée
    } else {
      // Exemple d'ajout de notifications
      const notification1 = {
        id: 1,
        type: 'success',
        header: 'Success',
        message: 'Operation completed successfully.'
      };

      const notification2 = {
        id: 2,
        type: 'error',
        header: 'Error',
        message: 'An error occurred.'
      };

      // this.addNotification(notification1);
      // this.addNotification(notification2);
    }
  }
};
</script>
