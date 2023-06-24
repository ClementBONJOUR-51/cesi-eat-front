<template>
    <transition name="notification">
      <div class="notification-container">
        <div class="notification notification-top" :class="type" v-if="isVisible">
          <div class="notification-content">
            <h3 class="notification-header">{{ header }}</h3>
            <p class="notification-message">{{ message }}</p>
          </div>
          <button class="notification-close" @click="closeNotification">X</button>
        </div>
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
        timer: null,
      };
    },
    methods: {
      closeNotification() {
        this.isVisible = false;
        this.$emit('closed');
      },
      startTimer() {
        clearTimeout(this.timer);
        this.timer = setTimeout(() => {
          this.isVisible = false;
          this.$emit('closed');
        }, this.duration);
      },
    },
    mounted() {
      if (this.duration > 0) {
        this.startTimer();
      }
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
    /* taille */
    width: 300px;
    margin-left: auto;
    margin-right: auto;
    position: relative;
  }
  
  .notification.success {
    border-color: #00ff00;
    background-color: #00ff00a3;
  }
  
  .notification.error {
    border-color: #ff0000;
    background-color: #ff0000a3;
  }
  
  .notification.warning {
    border-color: #ffa500;
    background-color: #ffa500a3;
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
  
  .notification-top {
    padding: 10px;
    padding-right: 40px;
  }
  
  .notification .notification-close {
    background: none;
    border: none;
    cursor: pointer;
    font-size: 16px;
    color: black;
    margin-left: 10px;
    position: absolute;
    top: 10px;
    right: 10px;
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
  