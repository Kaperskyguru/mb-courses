<!-- eslint-disable vue/no-v-html -->
<template>
  <div :style="[showImage ? { marginTop: '150px' } : '']" class="shadow">
    <div class="position-relative mt-5">
      <div
        class="bordder-primary text-center w-100"
        :style="
          showImage
            ? { maxWidth: '950px', height: '500px' }
            : { maxHeight: '300px' }
        "
      >
        <div v-if="showImage" style="width: 200px; margin: -100px auto 30px">
          <img
            class="hero-img rounded-circle img-fluid w-80"
            :src="image"
            :alt="name"
          />
        </div>

        <div
          class="p-lg-3 p-2 pt-10 d-flex flex-column justify-content-center align-items-center"
        >
          <div class="w-100">
            <p
              class="text lh-md text-black"
              style="font-size: 16px; font-weight: 500; text-align: left"
              :style="showImage ? { textAlign: 'left' } : {}"
              v-html="feedback"
            ></p>
          </div>
          <div
            class="d-flex pt-1 flex-row justify-content-center w-100"
            style="width: auto"
          >
            <p class="fs-6 fw-bold">
              {{ name }}
              <span class="fw-normal">. {{ position }}</span>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

defineOptions({ name: 'TestimonialComponent' })

// ✅ Props
const props = defineProps({
  showImage: {
    type: Boolean,
    default: true,
  },
  testimonial: {
    type: Object as () => {
      image?: string
      feedback?: string
      name?: string
      position?: string
    },
    default: () => ({}),
  },
})

// ✅ Computed properties
const image = computed(() =>
  props.testimonial?.image ? props.testimonial.image : '/img/backend-2023.jpg'
)

const feedback = computed(() => `"${props.testimonial?.feedback ?? ''}"`)

const name = computed(() => props.testimonial?.name ?? '')

const position = computed(() => props.testimonial?.position ?? '')
</script>

<style scoped>
h1 {
  color: #0a083b;
}

p {
  color: #57586e;
  font-size: 0.8rem;
}

.bordder-primary {
  border: 1px solid black;
  border-radius: 5px;
}

small {
  color: #57586e;
}
</style>
