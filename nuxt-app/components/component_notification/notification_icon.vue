<template>
      <div>
    <div class="notification-icon" @click="toggleNotifications">
      <i class="notification-icon__icon">...</i>
      <span v-if="unreadCount > 0" class="notification-badge">{{ unreadCount }}</span>
    </div>

    <!-- ... Reste du contenu ... -->

    <Notification v-if="showNotifications" :notifications="notifications" />
  </div>
  </template>
  
  <script>
  import Notification from '~/components/component_notification/notification.vue';
  
  export default {
    props: {
    unreadCount: {
      type: Number,
      default: 0
    }
    },
    data() {
      return {
        // unreadCount: 0,
        showNotifications: false,
        notifications: [],
      };
    },
    methods: {
//       toggleNotifications() {
//   this.showNotifications = !this.showNotifications;
//   if (this.showNotifications) {
//     this.$emit('update-notifications', this.notificationsData, this.unreadCount);
//   }
// },

toggleNotifications() {
  this.showNotifications = !this.showNotifications;
  if (this.showNotifications) {
    this.$emit('update-notifications', this.unreadCount);
  }
},

  
      // addNotificationIcon(notification) {
      //   this.notifications.push(notification);
      //   // this.unreadCount++;
      //   this.$emit('update-notifications', this.notifications.length);
      // },
    //   removeNotification(notification) {
    //     const index = this.notifications.indexOf(notification);
    //     if (index !== -1) {
    //       this.notifications.splice(index, 1);
    //       this.unreadCount--;
    //     }
    //   },
    },
    mounted() {
      // const newNotificationCount = 3;
      // this.unreadCount = newNotificationCount;
      // Incrémenter unreadCount avec l'ajout 
    },
  };
  </script>
  <style scoped>
  .notification-icon {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }
  
  .notification-badge {
    position: absolute;
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
  /* Styles de l'icône de la notification */
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
  