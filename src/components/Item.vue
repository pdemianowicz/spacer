<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  item: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div :style="'background-image: url(' + props.item.links[0].href + ')'" class="item"></div>
</template>

<style scoped>
.item {
  width: 40vw;
  height: 250px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50%;

  @media (min-width: 768px) {
    width: 25vw;
  }
}
</style>
