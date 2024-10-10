<template>
  <div class="users-container">
    <h1>Get All Users</h1>
    <div class="header-btn">
      <button class="create-btn" v-on:click="navigateTo('/user/create')">สร้างผู้ใช้</button>
    </div>
    <hr />
    <div v-if="users.length" class="user-list">
      <div class="user-count"><b>จำนวนผู้ใช้งาน:</b> {{ users.length }}</div>
      <div v-for="user in users" :key="user.id" class="user-card">
        <div><b>ID:</b> {{ user.id }}</div>
        <div><b>ชื่อผู้ใช้:</b> {{ user.name }} {{ user.lastname }}</div>
        <div><b>อีเมล:</b> {{ user.email }}</div>
        <div><b>สถานะ:</b> {{ user.status }}</div>
        <div><b>ประเภท:</b> {{ user.type }}</div>
        <div class="btn-group">
          <button class="info-btn" v-on:click="navigateTo('/user/'+user.id)">ดูข้อมูล</button>
          <button class="edit-btn" v-on:click="navigateTo('/user/edit/'+user.id)">แก้ไขข้อมูล</button>
          <button class="delete-btn" v-on:click="deleteUser(user)">ลบข้อมูล</button>
        </div>
      </div>
    </div>
    <div class="logout-btn">
      <button v-on:click="logout">Logout</button>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";
export default {
  data() {
    return {
      users: [],
    };
  },
  async created() {
    try {
      this.users = (await UsersService.index()).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    logout() {
      this.$store.dispatch("setToken", null);
      this.$store.dispatch("setUser", null);
      this.$router.push({
        name: "login",
      });
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(user) {
      let result = confirm("คุณต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        try {
          await UsersService.delete(user);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style scoped>
/* ตั้งค่าพื้นหลังและการจัดเรียง */
.users-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background: linear-gradient(135deg, #355c7d, #6c5b7b, #c06c84, #f67280, #f8b195);
  font-family: Arial, sans-serif;
  color: #2f2f2f;
}

/* ปุ่มสร้างและออกจากระบบ */
.header-btn,
.logout-btn {
  margin: 10px 0;
}

.create-btn,
.logout-btn button {
  background: linear-gradient(90deg, #36d1dc, #5b86e5);
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s;
}

.create-btn:hover,
.logout-btn button:hover {
  background: linear-gradient(90deg, #5b86e5, #36d1dc);
}

/* การ์ดผู้ใช้ */
.user-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.user-card {
  background: #fff;
  border-radius: 10px;
  padding: 20px;
  margin: 15px;
  width: 80%;
  max-width: 500px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  color: #333;
  font-size: 16px;
}

.user-card div {
  margin-bottom: 5px;
}

/* กลุ่มปุ่มในการ์ด */
.btn-group {
  display: flex;
  justify-content: space-around;
  margin-top: 10px;
}

/* ปุ่มต่าง ๆ */
.info-btn,
.edit-btn,
.delete-btn {
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s ease;
}

.info-btn {
  background: #3498db;
}

.edit-btn {
  background: #f1c40f;
}

.delete-btn {
  background: #e74c3c;
}

.info-btn:hover {
  background: #2980b9;
}

.edit-btn:hover {
  background: #d4ac0d;
}

.delete-btn:hover {
  background: #c0392b;
}
</style>
