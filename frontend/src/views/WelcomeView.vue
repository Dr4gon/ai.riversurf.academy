<template>
  <div class="container">
    <div class="chat-container">
      <div class="chat-view">
        <form v-show="currentView === 'chat'" class="chat-window">
          <div class="chat-header">
            <h2 class="chat-title">Dein Surf-Coach</h2>
            <div class="wave-divider">
              <svg viewBox="0 0 200 20" preserveAspectRatio="none">
                <path d="M0 10 Q25 0 50 10 Q75 20 100 10 Q125 0 150 10 Q175 20 200 10" stroke="rgba(255,255,255,0.4)" stroke-width="1.5" fill="none"/>
              </svg>
            </div>
          </div>
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

        <SurferView v-show="currentView === 'surfer'" />
        <VideoView v-show="currentView === 'videos'" />
      </div>

      <nav class="bottom-nav">
        <button class="nav-item" :class="{ 'nav-item--active': currentView === 'chat' }" type="button" @click="currentView = 'chat'">
          <img src="@/assets/message-in-a-bottle.png" class="nav-icon-img" alt="Chat" />
          <span>Chat</span>
        </button>
        <button class="nav-item" :class="{ 'nav-item--active': currentView === 'surfer' }" type="button" @click="currentView = 'surfer'">
          <svg
            class="nav-icon-svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <circle cx="12" cy="8" r="4" />
            <path d="M4 20c0-4 3.6-7 8-7s8 3 8 7" />
          </svg>
          <span>Surfer</span>
        </button>
        <button class="nav-item" :class="{ 'nav-item--active': currentView === 'videos' }" type="button" @click="currentView = 'videos'">
          <svg
            class="nav-icon-svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <rect x="2" y="6" width="14" height="12" rx="2" />
            <path d="M16 10l6-3v10l-6-3V10z" />
          </svg>
          <span>Videos</span>
        </button>
      </nav>
      <footer class="attribution">
        Made by
        <a href="https://github.com/dr4gon" target="_blank" rel="noopener noreferrer" title="Dr4gon on GitHub">
          <svg
            viewBox="0 0 64 64"
            xmlns="http://www.w3.org/2000/svg"
            width="26"
            height="26"
            style="vertical-align: middle"
          >
            <path d="M19 26 Q13 10 20 5 Q22 17 27 23 Z" fill="#558B2F" />
            <path d="M45 26 Q51 10 44 5 Q42 17 37 23 Z" fill="#558B2F" />
            <ellipse cx="32" cy="38" rx="22" ry="20" fill="#8BC34A" />
            <ellipse cx="32" cy="29" rx="11" ry="7" fill="#7CB342" opacity="0.55" />
            <ellipse cx="32" cy="48" rx="13" ry="8" fill="#9CCC65" />
            <ellipse cx="22" cy="33" rx="6.5" ry="6.5" fill="#F1F8E9" />
            <ellipse cx="22" cy="33" rx="2.5" ry="5" fill="#1B5E20" />
            <circle cx="21" cy="31" r="1.2" fill="white" opacity="0.6" />
            <ellipse cx="42" cy="33" rx="6.5" ry="6.5" fill="#F1F8E9" />
            <ellipse cx="42" cy="33" rx="2.5" ry="5" fill="#1B5E20" />
            <circle cx="41" cy="31" r="1.2" fill="white" opacity="0.6" />
            <ellipse cx="26" cy="48" rx="3.5" ry="2.5" fill="#558B2F" opacity="0.75" />
            <ellipse cx="38" cy="48" rx="3.5" ry="2.5" fill="#558B2F" opacity="0.75" />
            <path d="M22 55 Q32 60 42 55" stroke="#558B2F" stroke-width="1.8" fill="none" stroke-linecap="round" />
          </svg>
        </a>
        with Open-Source ♥️ Contributions or feedback
        <a
          href="https://github.com/Dr4gon/riversurf.academy_frontend"
          target="_blank"
          rel="noopener noreferrer"
          title="GitHub"
        >
          <svg class="gh-icon" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M12 0C5.374 0 0 5.373 0 12c0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0 1 12 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z"
            />
          </svg>
        </a>
      </footer>
    </div>
  </div>
</template>

<script>
import { messageStore } from '../stores/message-store';
import { mapState, mapActions } from 'pinia';
import VideoView from './VideoView.vue';
import SurferView from './SurferView.vue';

export default {
  name: 'WelcomeView',
  components: { VideoView, SurferView },
  data() {
    return {
      userQuestion: '',
      initialMessage: import.meta.env.VITE_WELCOME_MSG,
      currentView: 'chat',
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
  border: 2px solid rgba(255, 255, 255, 0.5);
  border-bottom: none;
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
  overflow: hidden;
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
  overflow-y: auto;
  height: 100%;
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

.placeholder-view {
  width: 100%;
  height: 100%;
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
  align-items: stretch;
  height: 56px;
  background-color: var(--water-color);
  border-radius: 0 0 10px 10px;
  border: 2px solid rgba(255, 255, 255, 0.5);
  border-top: none;
  gap: 0;
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
  border-radius: 0;
  transition: color 0.2s, background-color 0.2s;
}

.nav-item:first-child {
  border-radius: 0 0 0 8px;
}

.nav-item:last-child {
  border-radius: 0 0 8px 0;
}

.nav-item:hover {
  color: #ffffff;
  background-color: rgba(255, 255, 255, 0.12);
}

.nav-item--active {
  color: #ffffff;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 0;
  box-shadow: 0 -3px 0 0 rgba(255, 255, 255, 0.9);
}

.nav-item:first-child.nav-item--active {
  border-radius: 0 0 0 8px;
}

.nav-item:last-child.nav-item--active {
  border-radius: 0 0 8px 0;
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

.attribution {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  gap: 8px;
  padding: 8px 0 12px;
  font-size: 12px;
  color: #ffffff;
}

.attribution a {
  display: inline-flex;
  align-items: center;
  color: var(--water-color);
  text-decoration: none;
  transition: opacity 0.2s;
}

.attribution a:last-child {
  color: #000000;
}

.attribution a:hover {
  opacity: 0.6;
}

.attribution svg {
  width: 22px;
  height: 22px;
}

.gh-icon {
  fill: #000000;
}

/* ── Chat header ─────────────────────────────────────────────── */
.chat-header {
  padding: 16px 20px 8px;
  text-align: center;
  flex-shrink: 0;
}

.chat-title {
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
