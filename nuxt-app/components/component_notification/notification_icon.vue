<template>
  <div>
    <div class="notification-icon flex justify-end m-5" @click="toggleNotifications">
      <div class="notification-icon__container">
        <i class="fa fa-bell notification-icon__icon" aria-hidden="true"></i>
        <span v-if="unreadCount > 0" class="notification-badge">{{ unreadCount }}</span>
      </div>
    </div>

    <div class="notification-list" v-if="showNotifications">
      <div v-for="notification in notifications" :key="notification.id" class="notification" :class="notification.type">
        <div class="notification-content">
          <div class="notification-header">{{ notification_PageVue.header }}</div>
          <div class="notification-message">{{ notification_PageVue.message }}</div>
        </div>
        <button class="notification-close" @click="removeNotification(notification_PageVue.id)">
          <i class="fas fa-times"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Notification from '~/components/component_notification/notification.vue';
import '@fortawesome/fontawesome-free/css/fontawesome.css';
import '@fortawesome/fontawesome-free/css/solid.css';
import '@fortawesome/fontawesome-free/css/regular.css';
import '@fortawesome/fontawesome-free/css/brands.css';
export default {
  components: {
    Notification
  },
  props: {
    unreadCount: {
      type: Number,
      default: 0
    },
    notification_PageVue: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      showNotifications: false,
      notifications: [],
    };
  },computed: {
  unreadNotifications() {
    return this.notifications.filter(notification => !notification.read);
  }
},created() {
  // Écouter l'événement 'notification-added' émis par la page
  this.$root.$emit('notification-added', (notifications) => {
    // Mettre à jour la prop notification_PageVue avec les nouvelles notifications
    this.notification_PageVue = [...this.notification_PageVue, ...notifications];
  });
},
  methods: {
    toggleNotifications() {
  this.$emit('toggle-notifications');
},
    // toggleNotifications() {
    //   this.showNotifications = !this.showNotifications;
    //   // recupérer fonction addNotification de notificationPage.vue
    //   // récupère les notifications de notificationPage.vue
    //   const notifications = this.notifications;
    // //   if (this.showNotifications) {
    // //     this.addNotification({
    // //       id: 1,
    // //       type: 'success',
    // //       header: 'Success',
    // //       message: 'Operation completed successfully111.'
    // //     });

    // //     this.addNotification({
    // //       id: 2,
    // //       type: 'error',
    // //       header: 'Error',
    // //       message: 'An error occurred.'
    // //     });
    // //   }
    // //   if (this.showNotifications) {
    // //     this.addNotification({
    // //       id: 1,
    // //       type: 'success',
    // //       header: 'Success',
    // //       message: 'Operation completed successfully111.'
    // //     });

    // //     this.addNotification({
    // //       id: 2,
    // //       type: 'error',
    // //       header: 'Error',
    // //       message: 'An error occurred.'
    // //     });
    // //   }
    // //   else{
    // //     // Si même notification alors on ne l'affiche pas
    // //     if (this.notifications = this.notifications) { // si notifications identiques
    // //       this.notifications = [];
    // //     }
    // //   }
    // },
    addNotification(notification) {
      this.notifications.push(notification);
    },
  }
};
</script>
<style scoped>
.notification-icon {
  position: relative;
  display: inline-block;
  cursor: pointer;
}

.notification-icon__container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.notification-badge {
  position: relative;
  top: -5px;
  right: -5px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  background-color: red;
  color: white;
  font-size: 12px;
  font-weight: bold;
  border-radius: 50%;
}

.notification {
  background-color: #fefefe;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 10px;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  animation: notification-fade-in 0.3s ease-in-out;
}

.notification.success {
  border-color: #00ff00;
}

.notification.error {
  border-color: #ff0000;
}

.notification.warning {
  border-color: #ffa500;
}

.notification .notification-content {
  flex-grow: 1;
}

.notification .notification-header {
  font-size: 16px;
  font-weight: bold;
}

.notification .notification-message {
  margin-top: 5px;
  font-size: 14px;
  color: #555;
}

.notification .notification-close {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  color: #555;
  margin-left: 10px;
}

.notification-icon__icon {
  /* position: relative; */
  font-size: 20px;
}


@keyframes notification-fade-in {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
  