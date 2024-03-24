<script setup>
import { ref, defineProps, defineEmits } from 'vue'
import debounce from 'lodash.debounce'

// Definicja propsów
const props = defineProps({
  value: String,
  dark: Boolean
})

// Definicja emisji zdarzeń
const emit = defineEmits(['update:value'])

const value = ref(props.value)

const searchValue = ref('')

const handleInput = debounce((e) => {
  const inputValue = e.target.value
  emit('update:value', inputValue)
}, 500)
</script>

<template>
  <input
    id="search"
    name="search"
    :class="{ dark: dark }"
    v-model="searchValue"
    @input="handleInput"
  />
</template>

<style scoped>
input {
  display: flex;
  flex-direction: column;
  width: 250px;
  margin-top: 50px;
  font-size: 18px;
  font-weight: 300;
  text-align: center;
  height: 30px;
  color: white;
  background: none;
  border: 0;
  border-bottom: 1px solid white;

  transition: box-shadow 0.3s ease-out;
}

input:focus {
  outline: none;
  box-shadow: 0 10px 20px -8px rgba(255, 255, 255, 0.5);
}

.dark {
  color: #1e3d4a;
  border-bottom-color: #1e3d4a;
}

.dark:focus {
  box-shadow: 0 10px 20px -8px rgba(30, 61, 74, 0.2);
}
</style>
