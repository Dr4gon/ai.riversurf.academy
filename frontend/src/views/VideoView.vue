<template>
  <div class="video-view about-background">
    <form class="video-upload" enctype="multipart/form-data">
      <h3>Lade dein Surf-Video für persönliches Feedback hoch</h3>
      <input type="text" placeholder="Wie heißt du?" v-model="name" />
      <input type="text" placeholder="Was willst du lernen?" v-model="goal" />
      <input type="text" placeholder="Wie ist deine Mailadresse?" v-model="email" />
      <input id="video" type="file" accept="video/*" />
      <button type="submit" @click.prevent="doUpload">Upload</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import FormData, { from } from 'form-data';

export default {
  name: 'AboutView',
  data() {
    return {
      name: '',
      goal: '',
      email: '',
    };
  },
  methods: {
    async doUpload() {
      console.log('Upload video');

      let video = document.getElementById('video').files[0];
      let form = new FormData();
      form.append('name', this.name);
      form.append('goal', this.goal);
      form.append('email', this.email);

      form.append('video', video);
      const response = await axios.post(import.meta.env.VITE_BACKEND_URL + '/upload/video', form, {
        headers: form.getHeaders ? form.getHeaders() : { 'Content-Type': 'multipart/form-data' }, // Apparently the browser switches to window.FormData which has no headers -> https://stackoverflow.com/questions/54682225/form-data-axios-unable-to-get-headers-from-formdata-error-getheaders-is-not
      });
    },
  },
};
</script>

<style scoped>
input {
  margin: 5px;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid var(--water-color);
}

button {
  margin: 5px;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid var(--water-color);
  background-color: var(--water-color);
  color: black;
}

.video-upload {
  display: flex;
  flex-direction: column;
  justify-content: start;
  padding-left: 10px;
  padding-right: 10px;
}


.video-view {
  width: 100%;
  height: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.about-background {
  background: linear-gradient(
    0deg,
    var(--underwater-color) 20%,
    var(--underwater-color) 30%,
    var(--water-color) 70%,
    var(--water-color) 80%
  );
  background-size: 100% 150%;
  animation: underwater 4s ease infinite;
}

@keyframes underwater {
  0% {
    background-position: 0 20%;
  }
  50% {
    background-position: 0 80%;
  }
  100% {
    background-position: 0 20%;
  }
}
</style>
