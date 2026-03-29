<template>
  <div class="surfer-view about-background">
    <div class="testimonials-section">
      <h3>Empfohlen durch Empfehlungen</h3>
      <div class="items" ref="slider">
        <div class="item">
          <div class="content">
            <div class="testimonials">
              <img class="testimonial-img" src="@/assets/surfer-girl.png" />
              <p>Vroni: "De Long ist mega gut die Kleinigkeiten zu entdecken und mich dadurch weiterzubringen."</p>
            </div>
            <audio controls src="src/assets/possibilities.mp3"></audio>
          </div>
        </div>
        <div class="item">
          <div class="content">
            <div class="testimonials">
              <img class="testimonial-img" src="@/assets/surfer-boy.png" />
              <p>
                Basti: "Surfen macht super viel Spaß. De Long ermutigt mich dabei dran zu bleiben und die Frustmomente
                gut zu überwinden."
              </p>
            </div>
            <audio controls src="src/assets/possibilities.mp3"></audio>
          </div>
        </div>
        <div class="item">
          <div class="content">
            <div class="testimonials">
              <img class="testimonial-img" src="@/assets/surfer-girl.png" />
              <p>
                Vicky: "Der Coach verrät mir immer wieder Tipps und Tricks, damit mir das Surfen Spaß macht. Dafür
                gibt's ne Kaffee oben druff."
              </p>
            </div>
            <audio controls src="src/assets/possibilities.mp3"></audio>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SurferView',
  mounted() {
    const slider = this.$refs.slider;
    let isDown = false;
    let startX;
    let scrollLeft;
    var velX = 0;
    var momentumID;

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

    slider.addEventListener('wheel', () => {
      cancelMomentumTracking();
    });

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
      if (Math.abs(velX) > 0.5) {
        momentumID = requestAnimationFrame(momentumLoop);
      }
    }
  },
};
</script>

<style scoped>
.surfer-view {
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
  0% { background-position: 0 20%; }
  50% { background-position: 0 80%; }
  100% { background-position: 0 20%; }
}

.testimonials-section {
  display: flex;
  flex-direction: column;
  padding: 10px;
}

.testimonials {
  display: flex;
  flex-direction: row;
  justify-content: start;
  margin-bottom: 5px;
}

.testimonial-img {
  width: 30vw;
}

.items {
  display: flex;
  gap: 8px;
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
  -ms-overflow-style: none;
  overflow-x: scroll;
  overflow-y: hidden;
  transition: all 0.2s;
  transform: scale(0.98);
  will-change: transform;
  user-select: none;
  cursor: pointer;
}

.items::-webkit-scrollbar {
  display: none;
}

.items.active {
  background: var(--water-color);
  cursor: grabbing;
  cursor: -webkit-grabbing;
  transform: scale(1);
}

.item {
  flex-shrink: 0;
  width: 100%;
}

.content {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
</style>
