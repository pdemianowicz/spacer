<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  item: {
    type: Object,
    required: true
  }
})
const photo = () => props.item.links[0].href
const title = () => props.item.data[0].title
const description = () => props.item.data[0].description.substring(0, 700)
</script>

<template>
  <div class="outerWrapper">
    <div class="innerWrapper">
      <div class="photo">
        <img :src="photo()" alt="" />
      </div>
      <div class="description">
        <h2 class="title">{{ title() }}</h2>
        <p>{{ description() }}</p>
      </div>
    </div>
    <div class="close" @click="$emit('closeModal')"></div>
  </div>
</template>

<style scoped>
.outerWrapper {
  max-width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: #f6f6f6;

  @media (min-width: 1024px) {
    max-width: 70%;
    height: 60%;
    inset: 0;
    margin: auto;
    box-shadow: 0 30px 30px -10px rgba(0, 0, 0, 0.3);
  }
}

.innerWrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  padding: 50px;

  @media (min-width: 1024px) {
    flex-direction: row;
  }
}

.photo {
  width: 100%;
  height: auto;
  background: black;

  @media (min-width: 1024px) {
    min-width: 50%;
    margin-right: 20px;
  }
}

.photo img {
  width: 100%;
}

.description {
  color: #333;
}

.close {
  position: absolute;
  width: 30px;
  height: 30px;
  padding: 30px;
  right: 0;
  top: 0;
  cursor: pointer;
}

.close::before,
.close::after {
  position: absolute;
  top: 30px;
  right: 20px;
  content: '';
  width: 20px;
  height: 2px;
  background: black;
  display: block;
}

.close::before {
  transform: rotate(45deg);
}

.close::after {
  transform: rotate(-45deg);
}
</style>
