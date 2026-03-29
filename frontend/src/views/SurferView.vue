<template>
  <div class="surfer-view">
    <div class="header">
      <h2 class="title">Stimmen aus dem Wasser</h2>
      <div class="wave-divider">
        <svg viewBox="0 0 200 20" preserveAspectRatio="none">
          <path d="M0 10 Q25 0 50 10 Q75 20 100 10 Q125 0 150 10 Q175 20 200 10" stroke="rgba(255,255,255,0.4)" stroke-width="1.5" fill="none"/>
        </svg>
      </div>
    </div>

    <div class="cards" ref="slider">
      <div class="card">
        <div class="card-bubble">
          <div class="quote-mark">"</div>
          <p class="quote-text">De Long ist mega gut die Kleinigkeiten zu entdecken und mich dadurch weiterzubringen.</p>
        </div>
        <div class="card-footer">
          <div class="avatar-wrap">
            <img src="@/assets/surfer-girl.png" class="avatar" alt="Vroni" />
            <div class="ripple"></div>
          </div>
          <div class="card-meta">
            <span class="name">Vroni</span>
            <audio controls src="src/assets/possibilities.mp3" class="audio"></audio>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-bubble">
          <div class="quote-mark">"</div>
          <p class="quote-text">Surfen macht super viel Spaß. De Long ermutigt mich dabei dran zu bleiben und die Frustmomente gut zu überwinden.</p>
        </div>
        <div class="card-footer">
          <div class="avatar-wrap">
            <img src="@/assets/surfer-boy.png" class="avatar" alt="Basti" />
            <div class="ripple"></div>
          </div>
          <div class="card-meta">
            <span class="name">Basti</span>
            <audio controls src="src/assets/possibilities.mp3" class="audio"></audio>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-bubble">
          <div class="quote-mark">"</div>
          <p class="quote-text">Der Coach verrät mir immer wieder Tipps und Tricks, damit mir das Surfen Spaß macht. Dafür gibt's ne Kaffee oben druff.</p>
        </div>
        <div class="card-footer">
          <div class="avatar-wrap">
            <img src="@/assets/surfer-girl.png" class="avatar" alt="Vicky" />
            <div class="ripple"></div>
          </div>
          <div class="card-meta">
            <span class="name">Vicky</span>
            <audio controls src="src/assets/possibilities.mp3" class="audio"></audio>
          </div>
        </div>
      </div>
    </div>

    <div class="dots">
      <span
        v-for="(_, i) in 3"
        :key="i"
        class="dot"
        :class="{ 'dot--active': activeIndex === i }"
        @click="scrollTo(i)"
      ></span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SurferView',
  data() {
    return { activeIndex: 0 };
  },
  mounted() {
    const slider = this.$refs.slider;
    let isDown = false;
    let startX;
    let scrollLeft;
    var velX = 0;
    var momentumID;

    slider.addEventListener('scroll', () => {
      const index = Math.round(slider.scrollLeft / slider.offsetWidth);
      this.activeIndex = index;
    });

    slider.addEventListener('mousedown', e => {
      isDown = true;
      slider.classList.add('active');
      startX = e.pageX - slider.offsetLeft;
      scrollLeft = slider.scrollLeft;
      cancelMomentumTracking();
    });
    slider.addEventListener('mouseleave', () => {
      isDown = false;
      slider.classList.remove('active');
    });
    slider.addEventListener('mouseup', () => {
      isDown = false;
      slider.classList.remove('active');
      beginMomentumTracking();
    });
    slider.addEventListener('mousemove', e => {
      if (!isDown) return;
      e.preventDefault();
      const x = e.pageX - slider.offsetLeft;
      const walk = (x - startX) * 3;
      var prevScrollLeft = slider.scrollLeft;
      slider.scrollLeft = scrollLeft - walk;
      velX = slider.scrollLeft - prevScrollLeft;
    });
    slider.addEventListener('wheel', () => cancelMomentumTracking());

    function beginMomentumTracking() {
      cancelMomentumTracking();
      momentumID = requestAnimationFrame(momentumLoop);
    }
    function cancelMomentumTracking() {
      cancelAnimationFrame(momentumID);
    }
    function momentumLoop() {
      slider.scrollLeft += velX;
      velX *= 0.95;
      if (Math.abs(velX) > 0.5) momentumID = requestAnimationFrame(momentumLoop);
    }
  },
  methods: {
    scrollTo(i) {
      this.$refs.slider.scrollTo({ left: i * this.$refs.slider.offsetWidth, behavior: 'smooth' });
    },
  },
};
</script>

<style scoped>
.surfer-view {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  color: white;
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

/* ── Cards slider ────────────────────────────────────────── */
.cards {
  display: flex;
  flex: 1;
  min-height: 0;
  overflow-x: scroll;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
  -ms-overflow-style: none;
  cursor: grab;
  padding: 8px 16px 12px;
  gap: 16px;
}

.cards::-webkit-scrollbar { display: none; }
.cards.active { cursor: grabbing; }

/* ── Single card ─────────────────────────────────────────── */
.card {
  flex-shrink: 0;
  width: calc(100% - 32px);
  scroll-snap-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 16px;
}

.card-bubble {
  position: relative;
  background: rgba(255, 255, 255, 0.12);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 16px 16px 16px 4px;
  padding: 18px 18px 14px;
  backdrop-filter: blur(4px);
}

.quote-mark {
  position: absolute;
  top: -10px;
  left: 14px;
  font-size: 3rem;
  line-height: 1;
  color: var(--underwater-color);
  font-family: Georgia, serif;
}

.quote-text {
  font-size: 0.92rem;
  line-height: 1.55;
  color: rgba(255, 255, 255, 0.9);
  margin: 0;
}

/* ── Card footer: avatar + name + audio ──────────────────── */
.card-footer {
  display: flex;
  align-items: center;
  gap: 14px;
  padding-left: 4px;
}

.avatar-wrap {
  position: relative;
  flex-shrink: 0;
}

.avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid var(--underwater-color);
  display: block;
}

.ripple {
  position: absolute;
  inset: -6px;
  border-radius: 50%;
  border: 1.5px solid rgba(64, 195, 180, 0.45);
  animation: ripple 2.5s ease-out infinite;
}

@keyframes ripple {
  0%   { transform: scale(0.9); opacity: 0.6; }
  100% { transform: scale(1.35); opacity: 0; }
}

.card-meta {
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex: 1;
  min-width: 0;
}

.name {
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.06em;
  color: var(--underwater-color);
  text-transform: uppercase;
}

.audio {
  width: 100%;
  height: 28px;
  filter: invert(1) brightness(0.8);
}

/* ── Dot indicators ──────────────────────────────────────── */
.dots {
  display: flex;
  justify-content: center;
  gap: 8px;
  padding: 8px 0 12px;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.4);
  cursor: pointer;
  transition: background 0.3s, transform 0.3s;
}

.dot--active {
  background: var(--underwater-color);
  border-color: var(--underwater-color);
  transform: scale(1.3);
}
</style>
