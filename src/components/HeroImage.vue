<script setup></script>

<template>
  <div class="heroImage"></div>
</template>

<style scoped>
.heroImage {
  position: fixed;
  top: 0;
  z-index: -1;
  width: 100%;
  height: 100vh;
  background-image: url('../assets/heroimage.jpg');
  background-repeat: no-repeat;
  background-position: 80% 0%;
  background-size: cover;
}

.heroImage::after {
  content: '';
  background: rgba(0, 0, 0, 0.2);
  position: absolute;
  inset: 0 0;
  z-index: 1;
  width: 100%;
  height: 100vh;
}
</style>
