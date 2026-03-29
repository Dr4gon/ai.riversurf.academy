<template>
  <div class="video-view">
    <div class="header">
      <h2 class="title">Dein Surf-Video</h2>
      <div class="wave-divider">
        <svg viewBox="0 0 200 20" preserveAspectRatio="none">
          <path
            d="M0 10 Q25 0 50 10 Q75 20 100 10 Q125 0 150 10 Q175 20 200 10"
            stroke="rgba(255,255,255,0.4)"
            stroke-width="1.5"
            fill="none"
          />
        </svg>
      </div>
    </div>

    <form class="upload-card" enctype="multipart/form-data">
      <p class="subtitle">Lade dein Video hoch und erhalte persönliches Feedback von deinem Coach.</p>

      <div class="field">
        <label>Dein Name</label>
        <input type="text" placeholder="Wie heißt du?" v-model="name" />
      </div>
      <div class="field">
        <label>Dein Ziel</label>
        <input type="text" placeholder="Was willst du lernen?" v-model="goal" />
      </div>
      <div class="field">
        <label>Deine E-Mail</label>
        <input type="text" placeholder="Wie ist deine Mailadresse?" v-model="email" />
      </div>

      <div class="file-field">
        <label for="video" class="file-label">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <polyline points="16 16 12 12 8 16" />
            <line x1="12" y1="12" x2="12" y2="21" />
            <path d="M20.39 18.39A5 5 0 0 0 18 9h-1.26A8 8 0 1 0 3 16.3" />
          </svg>
          <span>{{ fileName }}</span>
        </label>
        <input id="video" type="file" accept="video/*" @change="onFileChange" />
      </div>

      <button type="submit" @click.prevent="doUpload">
        <svg
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <line x1="22" y1="2" x2="11" y2="13" />
          <polygon points="22 2 15 22 11 13 2 9 22 2" />
        </svg>
        Absenden
      </button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import FormData from 'form-data';

export default {
  name: 'AboutView',
  data() {
    return {
      name: '',
      goal: '',
      email: '',
      fileName: 'Video auswählen…',
    };
  },
  methods: {
    onFileChange(e) {
      this.fileName = e.target.files[0]?.name ?? 'Video auswählen…';
    },
    async doUpload() {
      console.log('Upload video');
      let video = document.getElementById('video').files[0];
      let form = new FormData();
      form.append('name', this.name);
      form.append('goal', this.goal);
      form.append('email', this.email);
      form.append('video', video);
      await axios.post(import.meta.env.VITE_BACKEND_URL + '/upload/video', form, {
        headers: form.getHeaders ? form.getHeaders() : { 'Content-Type': 'multipart/form-data' },
      });
    },
  },
};
</script>

<style scoped>
.video-view {
  width: 100%;
  height: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  color: white;
  scrollbar-gutter: stable;
}

/* ── Header ─────────────────────────────────────────────── */
.header {
  padding: 16px 20px 8px;
  text-align: center;
}

.title {
  font-size: 1.1rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.9);
  margin: 0 0 8px;
}

.wave-divider {
  width: 100%;
  height: 16px;
  opacity: 0.6;
}

.wave-divider svg {
  width: 100%;
  height: 100%;
}

/* ── Upload card ─────────────────────────────────────────── */
.upload-card {
  margin: 0 16px 16px;
  padding: 20px;
  background: var(--water-color);
  border: none;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.subtitle {
  font-size: 0.82rem;
  color: rgba(255, 255, 255, 1);
  margin: 0 0 4px;
  line-height: 1.5;
}

/* ── Fields ──────────────────────────────────────────────── */
.field {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

label {
  font-size: 0.7rem;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  color: var(--underwater-color);
}

input[type='text'] {
  padding: 8px 12px;
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.4);
  background: rgba(255, 255, 255, 0.15);
  color: white;
  font-size: 0.9rem;
  outline: none;
  box-shadow: none;
  appearance: none;
  -webkit-appearance: none;
  transition: border-color 0.2s;
}

input[type='text']:focus {
  border-color: var(--underwater-color);
}

input[type='text']::placeholder {
  color: rgba(255, 255, 255, 0.65);
}

/* ── File picker ─────────────────────────────────────────── */
.file-field {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

input[type='file'] {
  display: none;
}

.file-label {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 12px;
  border-radius: 8px;
  border: 1px dashed rgba(255, 255, 255, 0.5);
  background: rgba(255, 255, 255, 0.1);
  color: white;
  font-size: 0.85rem;
  cursor: pointer;
  transition: border-color 0.2s, background 0.2s;
  text-transform: none;
  letter-spacing: normal;
}

.file-label:hover {
  border-color: var(--underwater-color);
  background: rgba(64, 195, 180, 0.1);
  color: white;
}

.file-label svg {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  color: var(--underwater-color);
}

/* ── Submit button ───────────────────────────────────────── */
button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 4px;
  padding: 10px 20px;
  border-radius: 8px;
  border: none;
  background-color: var(--underwater-color);
  color: white;
  font-size: 0.9rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  cursor: pointer;
  transition: opacity 0.2s;
}

button svg {
  width: 16px;
  height: 16px;
}

button:hover {
  opacity: 0.85;
}
</style>
