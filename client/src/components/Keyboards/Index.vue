<template>
    <div class="keyboard-container">
        <h2>Get all keyboards</h2>
        <div class="button-group">
            <button @click="logout" class="logout-btn" aria-label="Logout">Logout</button>
            <button @click="navigateTo('/keyboard/create')" class="create-btn" aria-label="Create Keyboard">สร้าง keyboard</button>
        </div>
        <h4>จำนวน keyboards: {{ keyboards.length }}</h4>
        <p v-if="loading" class="loading-text">Loading keyboards...</p>
        <div v-else v-for="keyboard in keyboards" :key="keyboard.id" class="keyboard-card">
            <p><strong>ID:</strong> {{ keyboard.id }}</p>
            <p><strong>Name:</strong> {{ keyboard.keyboard }}</p>
            <p><strong>Type:</strong> {{ keyboard.keyboardtype }}</p>
            <p><strong>Price:</strong> {{ keyboard.price }}</p>
            <p><strong>Status:</strong> {{ keyboard.status }}</p>
            <div v-if="keyboard.thumbnail" class="thumbnail">
                <img :src="BASE_URL + keyboard.thumbnail" alt="thumbnail" />
            </div>
            <div class="button-group">
                <button @click="navigateTo('/keyboard/' + keyboard.id)" class="view-btn" aria-label="View Keyboard">ดู keyboard</button>
                <button @click="navigateTo('/keyboard/edit/' + keyboard.id)" class="edit-btn" aria-label="Edit Keyboard">แก้ไข keyboard</button>
                <button @click="deleteKeyboard(keyboard)" class="delete-btn" aria-label="Delete Keyboard">ลบข้อมูล</button>
            </div>
            <hr />
        </div>
    </div>
</template>

<script>
import KeyboardsService from '@/services/KeyboardsService'
export default {
    data() {
        return {
            keyboards: [],
            loading: true,
            BASE_URL: "http://localhost:8081/assets/uploads/" // URL ที่เก็บรูปภาพที่อัปโหลด
        }
    },
    async created() {
        try {
            this.keyboards = (await KeyboardsService.index()).data;
        } catch (error) {
            console.error("Error loading keyboards:", error);
        } finally {
            this.loading = false;
        }
    },
    methods: {
        logout() {
            this.$store.dispatch('setToken', null)
            this.$store.dispatch('setKeyboard', null)
            this.$router.push({ name: 'login' })
        },
        navigateTo(route) {
            this.$router.push(route)
        },
        async deleteKeyboard(keyboard) {
            let result = confirm("Do you want to delete this keyboard?")
            if (result) {
                try {
                    await KeyboardsService.delete(keyboard)
                    this.refreshData()
                } catch (err) {
                    console.error("Error deleting keyboard:", err)
                }
            }
        },
        async refreshData() {
            this.loading = true;
            try {
                this.keyboards = (await KeyboardsService.index()).data;
            } finally {
                this.loading = false;
            }
        }
    }
}
</script>

<style scoped>
.keyboard-container {
    padding: 20px;
    background: linear-gradient(135deg, #6a85b6, #bac8e0);
    color: #2f2f2f;
    border-radius: 10px;
    box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
    max-width: 800px;
    margin: auto;
    font-family: Arial, sans-serif;
}

h2 {
    color: #fff;
    text-align: center;
    font-size: 32px;
    margin-bottom: 20px;
}

h4 {
    color: #333;
    font-weight: bold;
    text-align: center;
}

.loading-text {
    text-align: center;
    color: #333;
    font-weight: bold;
    margin-bottom: 20px;
}

.button-group {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

.logout-btn,
.create-btn,
.view-btn,
.edit-btn,
.delete-btn {
    background: linear-gradient(90deg, #36d1dc, #5b86e5);
    color: #fff;
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    margin: 5px;
    transition: background 0.3s;
}

.logout-btn:hover,
.create-btn:hover,
.view-btn:hover,
.edit-btn:hover,
.delete-btn:hover {
    background: linear-gradient(90deg, #5b86e5, #36d1dc);
}

.keyboard-card {
    background-color: #fff;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
    margin: 20px 0;
}

.keyboard-card p {
    margin: 5px 0;
    color: #555;
}

.thumbnail img {
    width: 100px;
    height: auto;
    border-radius: 5px;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
}
</style>
