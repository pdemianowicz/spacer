<script setup>
import { ref, defineProps } from 'vue'
import debounce from 'lodash.debounce'
import Claim from '@/components/Claim.vue'
import SearchInput from '@/components/SearchInput.vue'
import HeroImage from '@/components/HeroImage.vue'
import Item from '@/components/Item.vue'
import Modal from '@/components/Modal.vue'

const API = 'https://images-api.nasa.gov/search'
const searchValue = ref('')
const results = ref([])
const loading = ref(false)
const step = ref(0)
const modalOpen = ref(false)
const modalItem = ref(null)

const props = defineProps({
  searchValue: String //
})

const handleInput = debounce((e) => {
  const inputValue = e
  loading.value = true
  fetch(`${API}?q=${inputValue}&media_type=image`)
    .then((response) => response.json())
    .then((data) => {
      results.value = data.collection.items
      loading.value = false
      step.value = 1
    })
    .catch((error) => console.error(error))
}, 500)

const handleModalOpen = (item) => {
  modalOpen.value = true
  modalItem.value = item
}
</script>

<template>
  <main>
    <div :class="[{ flexStart: step === 1 }, 'wrapper']">
      <transition name="slide">
        <img src="../assets/logo.svg" alt="Logo" class="logo" v-if="step === 1" />
      </transition>
      <transition name="fade">
        <HeroImage v-if="step === 0" />
      </transition>
      <Claim v-if="step === 0" />
      <SearchInput v-model="searchValue" @update:value="handleInput" :dark="step === 1" />
      <div class="results" v-if="results && !loading && step === 1">
        <template v-if="results.length > 0">
          <Item
            v-for="item in results"
            :item="item"
            :key="item.data[0]"
            @click.native="handleModalOpen(item)"
          />
        </template>
        <template v-else>
          <p style="text-align: center">
            No results for this query. Try entering something related to space.
          </p>
        </template>
      </div>
      <div class="loader" v-if="step === 1 && loading"></div>
      <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = false" />
    </div>
  </main>
</template>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 30px;
  width: 100%;
  min-height: 100vh;
  position: relative;
}

.wrapper.flexStart {
  justify-content: flex-start;
}

.logo {
  position: absolute;
  top: 30px;
}

.results {
  margin-top: 50px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;

  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr 1fr;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
  transition: margin-top 0.3s ease;
}
.slide-enter,
.slide-leave-to {
  margin-top: -50px;
}

.loader {
  margin-top: 100px;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: inline-block;
  border-top: 3px solid #333;
  border-right: 3px solid transparent;
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
