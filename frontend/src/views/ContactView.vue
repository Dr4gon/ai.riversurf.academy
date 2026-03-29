<template>
  <div class="container contact-background">
    <div class="view">
      <form class="contact" id="contact-form" @submit.prevent="submit"></form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {};
</script>

<style scoped>
.contact-background {
  background: var(--underwater-color);
  background: linear-gradient(0deg, rgba(0, 0, 0, 1) 1%, var(--underwater-color) 100%);
}
</style>
