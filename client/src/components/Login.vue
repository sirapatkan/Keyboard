<template>
  <div class="login-container">
    <div class="login-card">
      <h1>User Login</h1>
      <form v-on:submit.prevent="onLogin">
        <div class="input-group">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" v-model="email" required />
        </div>
        <div class="input-group">
          <label for="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            v-model="password"
            required
          />
        </div>
        <button type="submit" class="login-button">Login</button>
        <div class="error" v-if="error">{{ error }}</div>
      </form>
    </div>
  </div>
</template>

<script>
import AuthenService from "../services/AuthenService";
export default {
  data() {
    return {
      email: "",
      password: "",
      error: null,
    };
  },
  methods: {
    async onLogin() {
      try {
        const response = await AuthenService.login({
          email: this.email,
          password: this.password,
        });

        this.$store.dispatch("setToken", response.data.token);
        this.$store.dispatch("setUser", response.data.user);

        this.$router.push({
          name: "users",
        });
      } catch (error) {
        console.log(error);
        this.error = error.response.data.error;
        this.email = "";
        this.password = "";
      }
    },
  },
};
</script>

<style scoped>
.login-container {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #355c7d, #6c5b7b, #c06c84, #f67280, #f8b195); /* เพิ่มไล่สีหลายระดับ */
  font-family: Arial, sans-serif;
}

.login-card {
  background: linear-gradient(135deg, #ffffff, #f8f9fa); /* ไล่สีอ่อน ๆ ภายในการ์ด */
  width: 350px;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0px 8px 25px rgba(0, 0, 0, 0.3); /* เพิ่มเงาเข้มขึ้น */
  text-align: center;
}

h1 {
  color: #4d4d4d;
  margin-bottom: 1.5rem;
  font-weight: 600;
  font-size: 24px;
}

.input-group {
  margin-bottom: 1.5rem;
  text-align: left;
}

label {
  display: block;
  font-weight: bold;
  color: #5f5f5f;
  margin-bottom: 0.5rem;
  font-size: 14px;
}

input[type="email"],
input[type="password"] {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  color: #333;
  border: 1px solid #d1d1d1;
  border-radius: 5px;
  transition: all 0.3s ease;
}

input[type="email"]:focus,
input[type="password"]:focus {
  outline: none;
  border-color: #f67280;
  box-shadow: 0 0 10px rgba(246, 114, 128, 0.4);
}

.login-button {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  font-weight: bold;
  color: #ffffff;
  background: linear-gradient(90deg, #f67280, #c06c84); /* ไล่สีปุ่ม */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.login-button:hover {
  background: linear-gradient(90deg, #c06c84, #6c5b7b);
}

.error {
  color: #e74c3c;
  font-size: 14px;
  margin-top: 1rem;
}
</style>
