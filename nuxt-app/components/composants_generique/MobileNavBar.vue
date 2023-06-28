<template>
    <div
        class="fixed bottom-0 z-50 w-full -translate-x-1/2 bg-white border-t border-gray-200 left-1/2 dark:bg-gray-700 dark:border-gray-600">
        <div class="grid h-full max-w-lg grid-cols-5 mx-auto">
            <NuxtLink data-tooltip-target="tooltip-home" type="button" class="inline-flex flex-col items-center justify-center p-4 hover:bg-gray-50 dark:hover:bg-gray-800 group" to="/Accueil">
                <svg class="w-6 h-6 mb-1 text-gray-500 dark:text-gray-400 group-hover:text-emerald-400 " fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                    <path d="M10.707 2.293a1 1 0 00-1.414 0l-7 7a1 1 0 001.414 1.414L4 10.414V17a1 1 0 001 1h2a1 1 0 001-1v-2a1 1 0 011-1h2a1 1 0 011 1v2a1 1 0 001 1h2a1 1 0 001-1v-6.586l.293.293a1 1 0 001.414-1.414l-7-7z"></path>
                </svg>
                <span class="sr-only">Home</span>
            </NuxtLink>
            <div id="tooltip-home" role="tooltip"
                class="absolute z-10 invisible inline-block px-3 py-2 text-sm font-medium text-white transition-opacity duration-300 bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700">
                Home
                <div class="tooltip-arrow" data-popper-arrow></div>
            </div>
            <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
                <div class="bg-white-400 rounded p-0">
                    <button @click="showNotifications = !showNotifications" :class="{
                        'text-red-500': hasUnreadNotifications || hasNewNotifications,
                        'text-gray-500': !hasUnreadNotifications && !hasNewNotifications && notifications.length > 0,
                        'text-gray-700': notifications.length === 0,
                        'p-9': true,
                    }">
                        <i class="fa fa-bell fa-lg"></i>
                        <span v-if="hasNewNotifications" class="new-notification">Nouvelle(s)</span>
                    </button>
                    <div style="position: absolute; left: 30px; right: 30px; bottom : 100px" class="">
                        <div v-if="showNotifications" class="bg-emerald-400 rounded p-4 border-b border-emerald-400"
                            v-for="notification in notifications" :key="notification._id"
                            :class="['bg-emerald-400', 'rounded', 'p-4', { 'bg-white': notification.read }]">
                            <h2 class="font-bold mb-2" :class="{ 'text-white': !notification.read }">{{
                                parseMessage(notification.message).Titre }}</h2>
                            <p :class="{ 'text-white': !notification.read }">{{ parseMessage(notification.message).message
                            }}
                            </p>
                            <button @click="markNotificationAsRead(notification)"
                                :class="['border-2', 'border-emerald-400', 'px-5', 'rounded', { 'border-white text-white': !notification.read }]">
                                Vu
                            </button>
                        </div>
                    </div>
                </div>
            </div>
            <div id="tooltip-bookmark" role="tooltip"
                class="absolute z-10 invisible inline-block px-3 py-2 text-sm font-medium text-white transition-opacity duration-300 bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700">
                Bookmark
                <div class="tooltip-arrow" data-popper-arrow></div>
            </div>
            <button data-tooltip-target="tooltip-post" type="button"
                class="inline-flex flex-col items-center justify-center p-4 hover:bg-gray-50 dark:hover:bg-gray-800 group">
                <svg class="w-6 h-6 mb-1 text-gray-500 dark:text-gray-400 group-hover:text-emerald-400" fill="currentColor"
                    viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                    <path clip-rule="evenodd" fill-rule="evenodd"
                        d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-11a1 1 0 10-2 0v2H7a1 1 0 100 2h2v2a1 1 0 102 0v-2h2a1 1 0 100-2h-2V7z">
                    </path>
                </svg>
                <span class="sr-only">New post</span>
            </button>
            <div id="tooltip-post" role="tooltip"
                class="absolute z-10 invisible inline-block px-3 py-2 text-sm font-medium text-white transition-opacity duration-300 bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700">
                New post
                <div class="tooltip-arrow" data-popper-arrow></div>
            </div>
            <button data-tooltip-target="tooltip-search" type="button"
                class="inline-flex flex-col items-center justify-center p-4 hover:bg-gray-50 dark:hover:bg-gray-800 group">
                <svg class="w-6 h-6 mb-1 text-gray-500 dark:text-gray-400 group-hover:text-emerald-400 " fill="currentColor"
                    viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                    <path clip-rule="evenodd" fill-rule="evenodd"
                        d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z">
                    </path>
                </svg>
                <span class="sr-only">Search</span>
            </button>
            <div id="tooltip-search" role="tooltip"
                class="absolute z-10 invisible inline-block px-3 py-2 text-sm font-medium text-white transition-opacity duration-300 bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700">
                Search
                <div class="tooltip-arrow" data-popper-arrow></div>
            </div>
            <NuxtLink data-tooltip-target="tooltip-settings" type="button"
                class="inline-flex flex-col items-center justify-center p-4 hover:bg-gray-50 dark:hover:bg-gray-800 group"
                to="../../pages_generique/settings">
                <svg class="w-6 h-6 mb-1 text-gray-500 dark:text-gray-400 group-hover:text-emerald-400 " fill="currentColor"
                    viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                    <path
                        d="M5 4a1 1 0 00-2 0v7.268a2 2 0 000 3.464V16a1 1 0 102 0v-1.268a2 2 0 000-3.464V4zM11 4a1 1 0 10-2 0v1.268a2 2 0 000 3.464V16a1 1 0 102 0V8.732a2 2 0 000-3.464V4zM16 3a1 1 0 011 1v7.268a2 2 0 010 3.464V16a1 1 0 11-2 0v-1.268a2 2 0 010-3.464V4a1 1 0 011-1z">
                    </path>
                </svg>
                <span class="sr-only">Settings</span>
            </NuxtLink>
            <div id="tooltip-settings" role="tooltip"
                class="absolute z-10 invisible inline-block px-3 py-2 text-sm font-medium text-white transition-opacity duration-300 bg-gray-900 rounded-lg shadow-sm opacity-0 tooltip dark:bg-gray-700">
                Settings
                <div class="tooltip-arrow" data-popper-arrow></div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, computed } from 'vue'
import axios from 'axios'
import jwt_decode from "jwt-decode";
import '@fortawesome/fontawesome-free/css/fontawesome.css';
import '@fortawesome/fontawesome-free/css/solid.css';
import '@fortawesome/fontawesome-free/css/regular.css';
import '@fortawesome/fontawesome-free/css/brands.css';

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
                // console.log(decoded_user_id.id);

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
            }, 30000); // 30 secondes
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


<style>/* .bell-icon {
  color: red; */
/* Changer la couleur de l'icône en rouge */
/*}*/

.new-notification {
    background-color: red;
    color: white;
    padding: 2px 4px;
    border-radius: 15%;
    font-size: 12px;
    margin-left: 4px;
}</style>