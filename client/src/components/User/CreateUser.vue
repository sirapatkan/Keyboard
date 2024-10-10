<template>
  <div class="create-user-container">
    <h1>Create User</h1>
    <form @submit.prevent="createUser" class="user-form">
      <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="user.name" required />
      </div>
      <div class="form-group">
        <label for="lastname">Lastname:</label>
        <input type="text" id="lastname" v-model="user.lastname" required />
      </div>
      <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="user.email" required />
      </div>
      <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="user.password" required />
      </div>
      <div class="button-group">
        <button type="submit" class="create-btn">สร้างผู้ใช้</button>
        <button type="button" class="back-btn" @click="goToIndex">กลับ</button>
      </div>
    </form>
  </div>
</template>

<script>
import UsersService from "../../services/UsersService";
export default {
  data() {
    return {
      user: {
        name: "",
        lastname: "",
        email: "",
        password: "",
        status: "active",
      },
    };
  },
  methods: {
    async createUser() {
      try {
        await UsersService.post(this.user);
        this.$router.push("/users");
      } catch (err) {
        console.log(err);
      }
    },
    goToIndex() {
      this.$router.push("/users");
    },
  },
};
</script>

<style scoped>
.create-user-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background: linear-gradient(135deg, #355c7d, #6c5b7b, #c06c84, #f67280, #f8b195);
  font-family: Arial, sans-serif;
  color: #2f2f2f;
}

h1 {
  color: #ffffff;
  font-size: 32px;
  margin-bottom: 20px;
}

.user-form {
  background-color: #fff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
  color: #555;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

.button-group {
  display: flex;
  justify-content: space-between;
}

.create-btn,
.back-btn {
  background: linear-gradient(90deg, #36d1dc, #5b86e5);
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s;
}

.create-btn:hover {
  background: linear-gradient(90deg, #5b86e5, #36d1dc);
}

.back-btn {
  background: linear-gradient(90deg, #f8b400, #fecd2f);
}

.back-btn:hover {
  background: linear-gradient(90deg, #fecd2f, #f8b400);
}
</style>
