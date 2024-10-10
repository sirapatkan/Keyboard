<template>
  <div class="show-user-container">
    <h1>Show User</h1>
    <div class="user-info">
      <p><strong>ID:</strong> {{ user.id }}</p>
      <p><strong>Name:</strong> {{ user.name }}</p>
      <p><strong>Lastname:</strong> {{ user.lastname }}</p>
      <p><strong>Email:</strong> {{ user.email }}</p>
      <p><strong>Status:</strong> {{ user.status }}</p>
      <p><strong>Type:</strong> {{ user.type }}</p>
      <p><strong>Created At:</strong> {{ user.createdAt }}</p>
    </div>
    <button @click="goBackToUsers" class="back-btn"> กลับ </button>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      user: {}
    };
  },
  async created() {
    try {
      const userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    goBackToUsers() {
      this.$router.push({ name: "users" });
    }
  }
};
</script>

<style scoped>
.show-user-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background: linear-gradient(135deg, #355c7d, #6c5b7b, #c06c84, #f67280, #f8b195);
  font-family: Arial, sans-serif;
  color: #2f2f2f;
  border-radius: 10px;
}

h1 {
  color: #ffffff;
  font-size: 32px;
  margin-bottom: 20px;
}

.user-info {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  color: #333;
}

.user-info p {
  margin: 10px 0;
  font-size: 16px;
}

.user-info strong {
  color: #555;
}

.back-btn {
  margin-top: 20px;
  background: linear-gradient(90deg, #36d1dc, #5b86e5);
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s;
}

.back-btn:hover {
  background: linear-gradient(90deg, #5b86e5, #36d1dc);
}
</style>
