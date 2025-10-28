<template>
  <div
    class="my-2"
    :class="{
      'p-3 border rounded': topic?.number === toggleNumber && show,
      'py-3': !(topic?.number === toggleNumber && show),
    }"
  >
    <div
      class="py-2 d-flex justify-content-between px-3 align-items-center shadow-sm"
    >
      <div class="d-flex flex-column">
        <span class="fw-bold" style="font-size: 0.875rem; color: rgb(82 82 91)"
          >Module {{ topic?.number }}</span
        >
        <span class="fs-3 fw-bold">{{ topic?.title }}</span>
      </div>
      <div @click="toggle(topic?.number)" :style="`color: ${color}`">
        <svg
          v-if="topic?.number === toggleNumber && show"
          xmlns="http://www.w3.org/2000/svg"
          width="22"
          height="22"
          viewBox="0 0 512 512"
          fill="currentColor"
        >
          <path
            d="M233.4 406.6c12.5 12.5 32.8 12.5 45.3 0l192-192c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L256 338.7 86.6 169.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l192 192z"
          />
        </svg>

        <svg
          v-else
          xmlns="http://www.w3.org/2000/svg"
          fill="currentColor"
          width="22"
          height="22"
          viewBox="0 0 512 512"
        >
          <path
            d="M233.4 105.4c12.5-12.5 32.8-12.5 45.3 0l192 192c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L256 173.3 86.6 342.6c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3l192-192z"
          />
        </svg>
      </div>
    </div>

    <div class="d-flex row pt-2" v-if="topic?.number === toggleNumber && show">
      <div
        class="col-12 col-md-6 p-2 col-lg-4"
        v-for="(content, i) in topic?.content"
        :key="i"
      >
        <Video
          :tag="tag"
          :color="color"
          :content="{ ...content, index: i + 1 }"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// ✅ Props
const props = defineProps({
  color: String,
  tag: String,
  topic: {
    type: Object,
    default: () => ({}),
  },
})

// ✅ State
const show = ref(true)
const toggleNumber = ref(1)

// ✅ Methods
const toggle = (number) => {
  toggleNumber.value = number
  show.value = !show.value
}
</script>

<style scoped>
</style>
