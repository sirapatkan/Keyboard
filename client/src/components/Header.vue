<template>
  <div>
    <div class="nv-navbar">
      <ul class="nav">
        <li><router-link :to="{ name: 'keyboards' }">Keyboards</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>

        <!-- ถ้าผู้ใช้ล็อกอินแล้วแสดงปุ่ม Logout แทนปุ่ม Login -->
        <li>
          <a v-if="isUserLoggedIn" href="#" @click.prevent="logout">Logout</a>
          <router-link v-else :to="{ name: 'login' }">Login</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    isUserLoggedIn() {
      return this.$store.getters.isUserLoggedIn; // ตรวจสอบสถานะการล็อกอิน
    },
  },
  methods: {
    logout() {
      this.$store.dispatch("logout");
      this.$router.push({ name: "login" });
    },
  },
};
</script>

<style scoped>
.nv-navbar {
  background: linear-gradient(90deg, #007BA7, #00B2EE);
  width: 100%;
  padding: 15px 0;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
  font-family: Arial, sans-serif;
}

.nv-navbar .nav {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

.nv-navbar .nav li {
  margin: 0 15px;
}

.nv-navbar .nav li a {
  padding: 10px 15px;
  text-decoration: none;
  color: #ffffff;
  font-weight: bold;
  font-size: 16px;
  transition: all 0.3s ease;
  border-radius: 5px;
}

.nv-navbar .nav li a:hover {
  background-color: #70DBDB;
  color: #ffffff;
  transform: scale(1.1);
}

.nv-navbar .nav li a.router-link-active {
  background-color: #4DB8FF;
  color: #ffffff;
}

.clearfix {
  clear: both;
}
</style>
