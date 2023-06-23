<template>
    <transition name="notification">
    <div class="notification" :class="type" v-if="isVisible">
        <div class="notification-content">
          <h3 class="notification-header">{{ header }}</h3>
          <p class="notification-message">{{ message }}</p>
        </div>
        <button class="notification-close" v-if="closable" @click="closeNotification">×</button>
      </div>
    </transition>
  </template>
  
  <script>
  export default {
    props: {
      type: String,
      header: String,
      message: String,
      duration: {
        type: Number,
        default: 5000, // Durée par défaut en millisecondes
      },
      closable: {
        type: Boolean,
        default: true, // Notification ferme manuellement par défaut
      },
    },
    data() {
      return {
        isVisible: true,
      };
    },
    mounted() {
      if (this.duration > 0) {
        setTimeout(() => {
          this.closeNotification();
        }, this.duration);
      }
    },
    methods: {
      closeNotification() {
        this.isVisible = false;
      },
    },
};
  </script>
  
  <style scoped>
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
  