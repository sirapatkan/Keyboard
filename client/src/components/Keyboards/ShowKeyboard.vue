<template>
    <div class="keyboard-show-container">
      <h1>Keyboard Details</h1>
      <div class="keyboard-details">
        <p><strong>ID:</strong> {{ keyboard.id }}</p>
        <p><strong>Name:</strong> {{ keyboard.keyboard }}</p>
        <p><strong>Type:</strong> {{ keyboard.keyboardtype }}</p>
        <p><strong>Price:</strong> {{ keyboard.price }}</p>
        <p><strong>Status:</strong> {{ keyboard.status }}</p>
      </div>
      <div class="button-group">
        <button @click="navigateTo('/keyboard/edit/' + keyboard.id)" class="edit-btn">แก้ไข keyboard</button>
        <button @click="navigateTo('/keyboards')" class="back-btn">กลับ</button>
      </div>
    </div>
  </template>
  
  <script>
  import KeyboardsService from '@/services/KeyboardsService';
  
  export default {
    data() {
      return {
        keyboard: null
      };
    },
    async created() {
      try {
        let keyboardId = this.$route.params.keyboardId;
        this.keyboard = (await KeyboardsService.show(keyboardId)).data;
      } catch (error) {
        console.log(error);
      }
    },
    methods: {
      navigateTo(route) {
        this.$router.push(route);
      },
    }
  };
  </script>
  
  <style scoped>
  .keyboard-show-container {
    padding: 20px;
    background: linear-gradient(135deg, #74c0fc, #a5d8ff); /* ไล่สีฟ้าอ่อน */
    color: #2f2f2f;
    border-radius: 10px;
    box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    margin: auto;
    font-family: Arial, sans-serif;
  }
  
  h1 {
    text-align: center;
    color: #ffffff;
  }
  
  .keyboard-details {
    margin: 20px 0;
  }
  
  .keyboard-details p {
    font-size: 16px;
    margin: 8px 0;
    color: #333;
  }
  
  .button-group {
    text-align: center;
    margin-top: 20px;
  }
  
  button {
    padding: 10px 15px;
    color: #ffffff;
    font-weight: bold;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s;
  }
  
  .edit-btn {
    background: #ff922b; /* สีส้ม */
    margin-right: 10px;
  }
  
  .edit-btn:hover {
    background: #fd7e14; /* สีส้มเข้มเมื่อ hover */
  }
  
  .back-btn {
    background: #ff922b; /* สีส้ม */
  }
  
  .back-btn:hover {
    background: #fd7e14; /* สีส้มเข้มเมื่อ hover */
  }
  </style>
  