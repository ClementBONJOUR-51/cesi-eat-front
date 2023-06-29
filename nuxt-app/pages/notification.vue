<template>
  <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
    <div class="bg-white-400 rounded p-0">
      <button @click="showNotifications = !showNotifications" 
      :class="{
          'text-red-500': hasUnreadNotifications || hasNewNotifications,
          'text-black': !hasUnreadNotifications && !hasNewNotifications && notifications.length > 0,
          'text-black': notifications.length === 0
        }">
  <i class="fa fa-bell"></i>
  <span v-if="hasNewNotifications" class="new-notification">new</span>
      </button>
      <div v-if="showNotifications" class="bg-emerald-400 rounded p-4" v-for="notification in notifications"
        :key="notification._id"
        :class="['bg-emerald-400', 'rounded', 'p-4', { 'bg-white text-black': notification.read }]">
        <h2 class="font-bold mb-2">{{ parseMessage(notification.message).Titre }}</h2>
        <p>{{ parseMessage(notification.message).message }}</p>
        <button @click="markNotificationAsRead(notification)">Marquer comme lu</button>
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, ref, computed } from 'vue';
import axios from 'axios'
import jwt_decode from "jwt-decode";
import '@fortawesome/fontawesome-free/css/fontawesome.css';
import '@fortawesome/fontawesome-free/css/solid.css';
import '@fortawesome/fontawesome-free/css/regular.css';
import '@fortawesome/fontawesome-free/css/brands.css';

// ...

export default {
  name: 'Notification',
  setup() {
    const notifications = ref([]);
    const showNotifications = ref(false);

    const parseMessage = (message) => {
      try {
        const parsedMessage = JSON.parse(message.replace(/'/g, '"'));
        return parsedMessage;
      } catch (error) {
        console.error(error);
        return {};
      }
    };

    const hasUnreadNotifications = computed(() => {
      return notifications.value.some(notification => !notification.read);
    });

    const hasNewNotifications = computed(() => {
      return notifications.value.some(notification => !notification.read);
    });

    const fetchData = async () => {
      try {
        const token = localStorage.getItem('authToken');
        const decoded_user_id = jwt_decode(token);
        console.log(decoded_user_id.id);

        const response = await axios.get(`http://localhost:3000/getAllNotificationByUserId/${decoded_user_id.id}`);
        const newNotifications = response.data.result.notifications;
        const hasNew = newNotifications.some(notification => !notification.read);

        // Mise à jour de la variable hasNewNotifications
        notifications.value = newNotifications;

        return newNotifications;
      } catch (error) {
        console.error(error);
        return [];
      }
    };

    const removeOrder = (notificationId) => {
      notifications.value = notifications.value.filter(notification => notification._id !== notificationId);
    };

    const markNotificationAsRead = async (notification) => {
      try {
        notification.read = true;
        // Faire une requête PUT vers l'API pour mettre à jour la notification
        await axios.put(`http://localhost:3000/updateNotification/${notification._id}`, { read: true });
      } catch (error) {
        console.error(error);
      }
    };

    onMounted(async () => {
      notifications.value = await fetchData();
      setInterval(async () => {
        notifications.value = await fetchData();
      }, 5000); // 30 secondes
    });

    return {
      notifications,
      markNotificationAsRead,
      removeOrder,
      parseMessage,
      showNotifications, // Ajout de la variable showNotifications à l'export
      hasUnreadNotifications,
      hasNewNotifications,
    };
  },
};

</script>

<style>
/* .bell-icon {
  color: red; */
/* Changer la couleur de l'icône en rouge */
/*}*/

.new-notification {
  background-color: red;
  color: white;
  padding: 2px 4px;
  border-radius: 50%;
  font-size: 12px;
  margin-left: 4px;
}
</style>