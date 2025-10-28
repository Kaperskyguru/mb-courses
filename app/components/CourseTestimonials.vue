<template>
  <section
    id="Articles"
    class="Article mb-5 text-center p-3 Dev-art my-5 container mx-auto"
  >
    <PageTitle>
      <template #title>
        <h3 class="sponsor-header-text my-5 text-uppercase">
          {{ title }}
        </h3>
      </template>
    </PageTitle>

    <div class="flex-column d-flex">
      <div class="w-100 mx-auto">
        <div class="masonry-grid">
          <div
            v-for="(testimonial, index) in allTestimonials"
            :key="index"
            class="gri"
            :style="{ gridRow: 'span ' + 3 * index }"
          >
            <CourseTestimonial
              :showImage="showImage"
              :testimonial="testimonial"
            />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

defineOptions({
  name: 'TestimonialComponent',
})

// ✅ Props
const props = defineProps({
  showImage: {
    type: Boolean,
    default: true,
  },
  title: {
    type: String,
    default: 'What students are saying about the course',
  },
  data: {
    type: Array as () => Array<any>,
    default: () => [],
  },
})

// ✅ Local testimonials
const testimonials = ref([
  {
    feedback:
      'I strongly recommend exploring Mastering Backend as a resource for your personal and/or professional growth.',
    name: 'Agoro, Adegbenga. B',
    position: 'CTO, Crenet',
    image: '/img/people/agoro.jpeg',
  },
  {
    feedback: `The practical examples and hands-on exercises were particularly beneficial. They not only reinforced the theoretical concepts but also allowed me to apply them in real-world scenarios. The inclusion of best practices and common pitfalls added a practical dimension to the learning process.`,
    name: 'Daniel Tinivella',
    position: 'Software Engineer, Globant',
    image: '/img/people/daniel.webp',
  },
  {
    feedback: `The course is an excellent resource for beginners. Your explanations of the basics are clear, making it easy for newcomers to grasp. I particularly enjoyed the task management application; it's a practical example that helps solidify the concepts.`,
    name: 'Eshan Shafeeq',
    position: 'Blockchain & Web3 Engineer, Cake Defi',
    image: '/img/people/eshan.jpeg',
  },
])

// ✅ Combine static + dynamic data
const allTestimonials = computed(() => [...testimonials.value, ...props.data])
</script>

<style scoped>
.masonry-grid {
  display: grid;
  justify-content: center;
  gap: 1px;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  grid-template-rows: masonry;
}

.gri {
  opacity: 1;
  padding-left: 7px;
  padding-right: 7px;
}
</style>
