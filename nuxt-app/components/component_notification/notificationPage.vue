<template>
    <div>
      <NotificationIcon :unreadCount="unreadCount" @click="toggleNotifications" @update-notifications="notificationsData = $event; "/>
  
      <!-- Reste du contenu -->
  
      <Notification v-if="showNotifications" v-for="notification in notifications" :key="notification.id"
        :type="notification.type" :header="notification.header" :message="notification.message"
        @click="removeNotification(notification.id)"/>
        <!-- @closed pour juste fermer -->
    </div>
  </template>
  
  <script>
  import NotificationIcon from '~/components/component_notification/notification_icon.vue';
  import Notification from '~/components/component_notification/notification.vue';
  
  export default {
    components: {
      NotificationIcon,
      Notification
    },
    data() {
      return {
        unreadCount: 0,
        notifications: [],
        showNotifications: true,
        // notificationsData: [] // Ajoutez cette ligne
      };
    },
    methods: {
      toggleNotifications() {
        this.showNotifications = !this.showNotifications;
      },
  //     addNotification(notification) {
  //   this.notifications.push(notification);
  //   this.notificationsData.push(notification); // Ajoutez cette ligne
  //   this.unreadCount++;
  //   // console.log('notif', this.unreadCount)
  // },
  addNotification(notification) {
    this.notifications.push(notification);
    this.unreadCount = this.notifications.length;
    console.log('notif', this.unreadCount);
  },
  
  updateUnreadCount() {
      this.unreadCount = this.notifications.length;
      console.log('notif', this.unreadCount)
    },
  
      // removeNotification(id) {
      //   const index = this.notifications.findIndex(notification => notification.id === id);
      //   if (index !== -1) {
      //     this.notifications.splice(index, 1);
      //     this.unreadCount--;
      //   }
      // },
      removeNotification(id) {
    const index = this.notifications.findIndex(notification => notification.id === id);
    if (index !== -1) {
      this.notifications.splice(index, 1);
      this.updateUnreadCount();
    }
  },
  },
    // mounted() {
    //   this.addNotification({
    //     id: 1,
    //     type: 'success',
    //     header: 'Success',
    //     message: 'Operation completed successfully.'
    //   });
  
    //   this.addNotification({
    //     id: 2,
    //     type: 'error',
    //     header: 'Error',
    //     message: 'An error occurred.'
    //   });
    // }
  
    //  this.mountedFunction();  <NotificationPage :mountedFunction="customMountedFunction" />  
    // props: {
    //   mountedFunction: {
    //     type: Function,
    //     required: true
    //   }
    // },
  };
  </script>