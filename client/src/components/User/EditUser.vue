<template>
  <div class="edit-user-container">
    <h1>Edit User</h1>
    <form v-on:submit.prevent="editUser" class="edit-form">
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
        <input type="text" id="email" v-model="user.email" required />
      </div>
      <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="user.password" required />
      </div>
      <div class="button-group">
        <button type="submit" class="submit-btn">แก้ไขข้อมูล</button>
        <button type="button" @click="goBackToUsers" class="back-btn">กลับ </button>
      </div>
    </form>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";
export default {
  data() {
    return {
      user: {
        name: "",
        lastname: "",
        email: "",
        password: "",
        status: "active"
      }
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
    async editUser() {
      try {
        await UsersService.put(this.user);
        this.$router.push("/users");
      } catch (err) {
        console.log(err);
      }
    },
    goBackToUsers() {
      this.$router.push({ name: "users" });
    }
  }
};
</script>

<style scoped>
.edit-user-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background: linear-gradient(135deg, #355c7d, #6c5b7b, #c06c84, #f67280, #f8b195);
  font-family: Arial, sans-serif;
  color: #2f2f2f;
  border-radius: 10px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  margin: auto;
}

h1 {
  color: #ffffff;
  font-size: 32px;
  margin-bottom: 20px;
}

.edit-form {
  width: 100%;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  color: #ffffff;
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border-radius: 5px;
  border: 1px solid #ccc;
  box-shadow: inset 0px 1px 3px rgba(0, 0, 0, 0.1);
  font-size: 16px;
}

.button-group {
  display: flex;
  justify-content: space-between;
}

.submit-btn,
.back-btn {
  background: linear-gradient(90deg, #36d1dc, #5b86e5);
  color: #ffffff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s;
  flex: 1;
  margin: 5px;
}

.submit-btn:hover {
  background: linear-gradient(90deg, #5b86e5, #36d1dc);
}

.back-btn:hover {
  background: linear-gradient(90deg, #ff8a65, #ff7e5f);
}
</style>
