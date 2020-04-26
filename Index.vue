<template>
  <div>
    <div class="container mt-10">
      <div class="card bg-white">
        <img style="" :src="image" alt="">
        <input @change="handleImage" class="custom-input" type="file" accept="image/*">
      </div>
    </div>
    <div class="mt-10" style="text-align: center">
      <h3>SERVER IMAGE</h3>
      <img :src="remoteUrl" alt="">
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'home',
  data() {
    return {
      image: '',
      remoteUrl: ''
    }
  },
  methods: {
    handleImage(e) {
      const selectedImage = e.target.files[0]; // get first file
      this.createBase64Image(selectedImage);
    },
    createBase64Image(fileObject) {
      const reader = new FileReader();

      reader.onload = (e) => {
        this.image = e.target.result;
        this.uploadImage();
      };
      reader.readAsDataURL(fileObject);
    },
    uploadImage() {
      const { image } = this;
      axios.post('http://127.0.0.1:8081/upload', { image })
        .then((response) => {
          this.remoteUrl = response.data.url;
        })
        .catch((err) => {
          return new Error(err.message);
        })
    }
  },
}
</script>
<style>
  @import './assets/style/main.css';
</style>
