<template>
  <div class="keyboard-edit-container">
    <h1>Edit Keyboard</h1>
    <form v-on:submit.prevent="editKeyboard" class="keyboard-form">
      <div class="input-group">
        <label>Keyboard Name:</label>
        <input type="text" v-model="keyboard.keyboard" />
      </div>

      <transition name="fade">
        <div class="thumbnail-pic" v-if="keyboard.thumbnail">
          <img :src="BASE_URL + keyboard.thumbnail" alt="thumbnail" />
        </div>
      </transition>

      <div class="dropbox">
        <input
          type="file"
          multiple
          :name="uploadFieldName"
          :disabled="isSaving"
          @change="filesChange($event.target.name, $event.target.files); fileCount = $event.target.files.length;"
          accept="image/*"
          class="input-file"
        />
        <p v-if="isInitial">Drag your file(s) here to begin<br />or click to browse</p>
        <p v-if="isSaving">Uploading {{ fileCount }} files...</p>
        <p v-if="isSuccess">Upload Successful.</p>
      </div>

      <div class="thumbnail-list">
        <transition-group tag="ul" class="pictures">
          <li v-for="picture in pictures" :key="picture.id">
            <img :src="BASE_URL + picture.name" alt="picture image" />
            <button @click.prevent="useThumbnail(picture.name)">Thumbnail</button>
            <button @click.prevent="delFile(picture)">Delete</button>
          </li>
        </transition-group>
      </div>

      <div class="input-group">
        <label><strong>Keyboard Type:</strong></label>
        <vue-ckeditor v-model.lazy="keyboard.keyboardtype" :config="config" @blur="onBlur($event)" @focus="onFocus($event)" />
      </div>
      
      <div class="input-group">
        <label>Price:</label>
        <input type="text" v-model="keyboard.price" />
      </div>

      <div class="input-group">
        <label>Status:</label>
        <input type="text" v-model="keyboard.status" />
      </div>

      <div class="button-group">
        <button type="submit" class="update-btn">อัปเดต Keyboard</button>
        <button type="button" @click="navigateTo('/keyboards')" class="back-btn">กลับ</button>
      </div>
    </form>
  </div>
</template>

<script>
import KeyboardsService from "@/services/KeyboardsService";
import VueCkeditor from "vue-ckeditor2";
import UploadService from "../../services/UploadService";

const STATUS_INITIAL = 0,
  STATUS_SAVING = 1,
  STATUS_SUCCESS = 2,
  STATUS_FAILED = 3;

export default {
  components: { VueCkeditor },
  data() {
    return {
      BASE_URL: "http://localhost:8081/assets/uploads/",
      error: null,
      uploadError: null,
      currentStatus: STATUS_INITIAL,
      uploadFieldName: "userPhoto",
      uploadedFileNames: [],
      pictures: [],
      pictureIndex: 0,
      keyboard: {
        name: "",
        thumbnail: null,
        pictures: [],
        type: "",
        price: "",
        status: "",
      },
      config: {
        toolbar: [["Bold", "Italic", "Underline", "Strike"]],
        height: 300,
      },
    };
  },
  methods: {
    async delFile(picture) {
      let result = confirm("Want to delete?");
      if (result) {
        let dataJSON = { filename: picture.name };
        await UploadService.delete(dataJSON);
        this.pictures = this.pictures.filter(p => p.id !== picture.id);
      }
    },
    stripHtmlTags(value) {
      return value.replace(/<\/?[^>]+(>|$)/g, "");
    },
    async editKeyboard() {
      this.keyboard.keyboardtype = this.stripHtmlTags(this.keyboard.keyboardtype);
      try {
        await KeyboardsService.put(this.keyboard);
        this.$router.push({ name: "keyboards" });
      } catch (err) {
        console.log(err);
      }
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async save(formData) {
      try {
        this.currentStatus = STATUS_SAVING;
        await UploadService.upload(formData);
        this.currentStatus = STATUS_SUCCESS;
        this.uploadedFileNames.forEach(uploadFilename => {
          if (!this.pictures.some(p => p.name === uploadFilename)) {
            this.pictureIndex++;
            this.pictures.push({
              id: this.pictureIndex,
              name: uploadFilename,
            });
          }
        });
        this.clearUploadResult();
      } catch (error) {
        console.log(error);
        this.currentStatus = STATUS_FAILED;
      }
    },
    filesChange(fieldName, fileList) {
      const formData = new FormData();
      if (!fileList.length) return;
      Array.from(fileList).forEach(file => {
        formData.append(fieldName, file, file.name);
        this.uploadedFileNames.push(file.name);
      });
      this.save(formData);
    },
    clearUploadResult() {
      setTimeout(() => this.reset(), 5000);
    },
    useThumbnail(filename) {
      this.keyboard.thumbnail = filename;
    },
    reset() {
      this.currentStatus = STATUS_INITIAL;
      this.uploadError = null;
      this.uploadedFileNames = [];
    },
  },
  computed: {
    isInitial() {
      return this.currentStatus === STATUS_INITIAL;
    },
    isSaving() {
      return this.currentStatus === STATUS_SAVING;
    },
    isSuccess() {
      return this.currentStatus === STATUS_SUCCESS;
    },
    isFailed() {
      return this.currentStatus === STATUS_FAILED;
    },
  },
  async created() {
    this.currentStatus = STATUS_INITIAL;
    try {
      let keyboardId = this.$route.params.keyboardId;
      this.keyboard = (await KeyboardsService.show(keyboardId)).data;
      this.pictures = JSON.parse(this.keyboard.pictures);
      this.pictureIndex = this.pictures.length;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style scoped>
.keyboard-edit-container {
  padding: 20px;
  background: linear-gradient(135deg, #8ec5fc, #e0c3fc);
  color: #2f2f2f;
  border-radius: 10px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  max-width: 800px;
  margin: auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #ffffff;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  font-weight: bold;
  color: #555;
}

input[type="text"], .input-file {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.dropbox {
  outline: 2px dashed grey;
  outline-offset: -10px;
  background: lemonchiffon;
  color: dimgray;
  padding: 10px 10px;
  min-height: 200px;
  position: relative;
  cursor: pointer;
}

.dropbox:hover {
  background: khaki;
}

.thumbnail-list {
  margin-top: 15px;
}

.pictures img {
  max-width: 180px;
  margin-right: 20px;
  margin-bottom: 10px;
}

.button-group {
  text-align: center;
  margin-top: 20px;
}

button {
  padding: 10px 15px;
  color: #240a6b;
  font-weight: bold;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s;
}

.update-btn {
  background: #36d1dc;
  margin-right: 10px;
}

.update-btn:hover {
  background: #5b86e5;
}

.back-btn {
  background: #ff8c42;
}

.back-btn:hover {
  background: #ff5a1f;
}
</style>
