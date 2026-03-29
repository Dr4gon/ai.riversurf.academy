<template>
  <div class="container">
    <div class="chat-container">
      <div class="chat-view">
        <form class="chat-window">
          <div class="chat-text">
            <div class="chat-message-autoscroll">
              <div v-if="this.processing" class="chat-message-assistant">
                <div class="dot-stretching"></div>
              </div>

              <div v-for="message in this.messages">
                <div v-if="message.role === 'user'" class="chat-message-user">
                  <span class="chat-message chat-message-maincolor">{{ message.content }}</span>
                </div>
                <div v-else class="chat-message-assistant">
                  <div class="chat-message chat-message-secondarycolor">
                    <span>{{ message.content }}</span>
                    <div class="tooltip">
                      <img
                        @click="this.markUnuseful(this.messages.indexOf(message))"
                        v-if="message.useful"
                        class="chat-message-downvote-deactivated"
                        src="@/assets/poop.png"
                      />
                      <img @click="this.markUseful(this.messages.indexOf(message))" v-else src="@/assets/poop.png" />
                      <span class="tooltiptext">Falls dir diese Antwort nicht hilft, einmal hier klicken</span>
                    </div>
                  </div>
                </div>
              </div>

              <div class="chat-message-assistant">
                <div class="chat-message chat-message-secondarycolor">
                  <span>{{ this.initialMessage }}</span>
                </div>
              </div>
            </div>
          </div>
          <div class="chat-actions">
            <input class="text" v-model="userQuestion" />
            <button class="chat-button" type="submit" @click.prevent="doAskRiversurfAssistant">
              <img class="chat-send-img" src="@/assets/message-in-a-bottle.png" />
            </button>
          </div>
        </form>
      </div>

      <nav class="bottom-nav">
        <button class="nav-item" type="button">
          <img src="@/assets/message-in-a-bottle.png" class="nav-icon-img" alt="Chat" />
          <span>Chat</span>
        </button>
        <button class="nav-item" type="button">
          <svg class="nav-icon-svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="8" r="4" />
            <path d="M4 20c0-4 3.6-7 8-7s8 3 8 7" />
          </svg>
          <span>Surfer</span>
        </button>
        <button class="nav-item" type="button">
          <svg class="nav-icon-svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="2" y="6" width="14" height="12" rx="2" />
            <path d="M16 10l6-3v10l-6-3V10z" />
          </svg>
          <span>Videos</span>
        </button>
      </nav>
    </div>
  </div>
</template>

<script>
import { messageStore } from '../stores/message-store';
import { mapState, mapActions } from 'pinia';

export default {
  name: 'WelcomeView',
  data() {
    return {
      userQuestion: '',
      initialMessage: import.meta.env.VITE_WELCOME_MSG,
    };
  },
  mounted() {
    this.loadMessageHistory();
  },
  computed: {
    ...mapState(messageStore, ['messages', 'processing']),
  },
  methods: {
    ...mapActions(messageStore, ['askRiversurfAssistant', 'loadMessageHistory', 'markUseful', 'markUnuseful']),
    doAskRiversurfAssistant() {
      this.askRiversurfAssistant(this.userQuestion);
      this.userQuestion = '';
    },
  },
};
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 20px;
  width: min(85vw, 1000px);
  height: calc(100vh - 40px);
}

.chat-view {
  background-color: rgba(128, 128, 128, 0.5);
  display: flex;
  justify-content: center;
  align-items: stretch;
  flex: 1;
  min-height: 0;
  border-radius: 10px 10px 0 0;
}

input {
  width: inherit;
  border-radius: 0 0 0 5px;
  border: none;
  outline: none;
}

p {
  margin-left: 6vw;
  margin-right: 6vw;
}

.center {
  margin-left: 30vw;
}

.chat-text {
  margin-top: 5px;
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 10px 10px 0 0;
  width: inherit;
  flex: 1;
  min-height: 0;
  overflow: auto; /* Add scrollbar, when needed */
}

.chat-message {
  display: flex;
  flex-direction: column; /* Needed for the poop button */
  border-radius: 5px;
  padding: 5px;
  color: white;
  margin-bottom: 5px;
  max-width: 75%;
}

.chat-message-autoscroll {
  overflow: auto;
  max-height: 100%;
  display: flex;
  flex-direction: column-reverse;
}
.chat-message-maincolor {
  background-color: var(--water-color);
}

.chat-message-secondarycolor {
  background-color: var(--underwater-color);
}

.chat-message-assistant {
  display: flex;
  justify-content: flex-start;
}

.chat-message-assistant img {
  width: 20px;
}
/* From https://stackoverflow.com/questions/609273/convert-an-image-to-grayscale-in-html-css */

.chat-message-downvote-deactivated {
  filter: gray; /* IE6-9 */
  -webkit-filter: grayscale(1); /* Google Chrome, Safari 6+ & Opera 15+ */
  filter: grayscale(1); /* Microsoft Edge and Firefox 35+ */
}

/** Adapted from here: https://www.w3schools.com/css/tryit.asp?filename=trycss_tooltip_right */
.tooltip {
  position: relative;
  display: inline-block;
}

.tooltip .tooltiptext {
  visibility: hidden;
  max-width: 75%;
  background-color: #fcd232;
  color: #ba6b46;
  text-align: center;
  border-radius: 6px;
  padding: 5px;

  /* Position the tooltip */
  position: absolute;
  z-index: 1;
  top: -5px;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}

.chat-message-user {
  display: flex;
  justify-content: flex-end;
}

.chat-actions {
  display: inline-flex;
  width: inherit;
}

.chat-window {
  width: 100%;
  width: -moz-available; /* WebKit-based browsers will ignore this. */
  width: -webkit-fill-available; /* Mozilla-based browsers will ignore this. */
  display: flex;
  flex-direction: column;
}

.chat-button {
  border-radius: 0 0 5px 0;
  border: none;
  background-color: #ffffff;
  padding: 0; /* Safari fix to restore full size of image within */
  min-width: 44px;
  min-height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.chat-send-img {
  width: 24px;
  display: flex;
}

/* Scroll bar stylings */
::-webkit-scrollbar {
  width: 5px;
  height: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: var(--lightestgrey);
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 5px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555;
}

/* Taken from https://codepen.io/nzbin/pen/GGrXbp */

/**
 * ==============================================
 * Dot Stretching
 * ==============================================
 */
.dot-stretching {
  margin-left: 25px;
  margin-top: 10px;
  position: relative;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: var(--underwater-color);
  color: var(--underwater-color);
  transform: scale(1.25, 1.25);
  animation: dot-stretching 2s infinite ease-in;
}
.dot-stretching::before,
.dot-stretching::after {
  content: '';
  display: inline-block;
  position: absolute;
  top: 0;
}
.dot-stretching::before {
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: var(--underwater-color);
  color: var(--underwater-color);
  animation: dot-stretching-before 2s infinite ease-in;
}
.dot-stretching::after {
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: var(--underwater-color);
  color: var(--underwater-color);
  animation: dot-stretching-after 2s infinite ease-in;
}

@keyframes dot-stretching {
  0% {
    transform: scale(1.25, 1.25);
  }
  50%,
  60% {
    transform: scale(0.8, 0.8);
  }
  100% {
    transform: scale(1.25, 1.25);
  }
}
@keyframes dot-stretching-before {
  0% {
    transform: translate(0) scale(0.7, 0.7);
  }
  50%,
  60% {
    transform: translate(-20px) scale(1, 1);
  }
  100% {
    transform: translate(0) scale(0.7, 0.7);
  }
}
@keyframes dot-stretching-after {
  0% {
    transform: translate(0) scale(0.7, 0.7);
  }
  50%,
  60% {
    transform: translate(20px) scale(1, 1);
  }
  100% {
    transform: translate(0) scale(0.7, 0.7);
  }
}

.bottom-nav {
  display: flex;
  justify-content: space-around;
  align-items: center;
  height: 56px;
  background-color: var(--water-color);
  border-radius: 0 0 10px 10px;
  padding: 0 8px;
  gap: 4px;
}

.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 3px;
  flex: 1;
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.75);
  font-family: var(--water-font-family);
  font-size: 0.65rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  cursor: pointer;
  padding: 8px 0;
  border-radius: 6px;
  transition: color 0.2s, background-color 0.2s;
}

.nav-item:hover {
  color: #ffffff;
  background-color: rgba(255, 255, 255, 0.12);
}

.nav-icon-img {
  width: 22px;
  height: 22px;
  object-fit: contain;
  filter: brightness(0) invert(1);
  opacity: 0.75;
  transition: opacity 0.2s;
}

.nav-item:hover .nav-icon-img {
  opacity: 1;
}

.nav-icon-svg {
  width: 22px;
  height: 22px;
}

/* ── Responsive breakpoints ─────────────────────────────────── */

/* Small phones (≤ 480px) */
@media (max-width: 480px) {
  .chat-container {
    margin: 8px;
    width: calc(100vw - 16px);
    height: calc(100vh - 16px);
  }

  .chat-message {
    max-width: 90%;
  }
}

/* Tablets (481px – 768px) */
@media (min-width: 481px) and (max-width: 768px) {
  .chat-container {
    margin: 12px;
    width: calc(100vw - 24px);
    height: calc(100vh - 24px);
  }

  .chat-message {
    max-width: 85%;
  }
}

/* Landscape-Modus auf mobilen Geräten */
@media (max-height: 500px) and (orientation: landscape) {
  .chat-container {
    margin: 5px;
    height: calc(100vh - 10px);
  }

  .bottom-nav {
    height: 48px;
  }
}
</style>
