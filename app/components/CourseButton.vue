<template>
  <Button
    :custom-style="{
      backgroundColor: color,
      color: '#fff',
      ...customStyle,
    }"
    size="large"
    class="text-white fs-6"
    :type="type"
    :link="link"
    :href="link"
    v-bind="attrs"
  >
    {{ title }}
  </Button>
</template>

<script setup lang="ts">
import { useAttrs } from 'vue'

// ✅ Props definition
const props = defineProps({
  title: {
    type: String,
    default: '',
  },
  customStyle: {
    type: [String, Object],
    default: '',
  },
  color: {
    type: String,
    default: '',
  },
  type: {
    type: String,
    default: 'link',
  },
  link: {
    type: String,
    default: '',
  },
})

// ✅ Capture all attributes and events (Nuxt 4 compatible)
const attrs = useAttrs()
</script>

<style scoped>
</style>
