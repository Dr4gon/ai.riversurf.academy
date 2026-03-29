<template>
  <div class="main-container">
    <div class="stay">
      <WelcomeView />
    </div>
  </div>
</template>

<script>
import WelcomeView from '@/views/WelcomeView.vue';

export default {
  name: 'App',
  components: {
    WelcomeView,
  },
  created() {
    this.checkAndSetUUID();
  },
  methods: {
    checkAndSetUUID() {
      if (!localStorage.getItem('userIdentifier')) {
        localStorage.setItem('userIdentifier', this.generateUUID());
      }
    },
    generateUUID() {
      return self.crypto.randomUUID(); // only works via https or locally
    },
  },
};
</script>

<style scoped lang="scss">
.main-container {
  background-color: var(--water-color);
  position: relative;
  overflow: hidden;

  &:before,
  &:after {
    content: '';
    position: absolute;
    left: 50%;
    min-width: 300vw;
    min-height: 300vw;
    background-color: #fcfff5;
    animation-name: rotate;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
  }

  &:before {
    bottom: 30vh;
    border-radius: 45%;
    animation-duration: 10s;
  }

  &:after {
    bottom: 24vh;
    opacity: 0.5;
    border-radius: 47%;
    animation-duration: 10s;
  }
}

@keyframes rotate {
  0% {
    transform: translate(-50%, 0) rotateZ(0deg);
  }
  50% {
    transform: translate(-50%, -2%) rotateZ(180deg);
  }
  100% {
    transform: translate(-50%, 0%) rotateZ(360deg);
  }
}

.stay {
  position: relative;
  z-index: 1;
}
</style>
