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

    <div class="cards">
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

  </div>
</template>

<script>
export default {
  name: 'SurferView',
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
  flex-direction: column;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 8px 16px 16px;
  scrollbar-gutter: stable;
  gap: 16px;
}

/* ── Single card ─────────────────────────────────────────── */
.card {
  display: flex;
  flex-direction: column;
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

</style>
