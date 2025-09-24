<template>
  <section class="bg-[#f7f5fa]">
    <div class="container mx-auto">
      <div class="flex flex-wrap">
        <!-- Left Text -->
        <div id="chapter" class="w-full md:w-7/12 relative">
          <h1 class="text-[52px] font-extrabold leading-[62px] py-4">
            {{ title }} for Backend Engineers
          </h1>

          <p class="text-xl">{{ description }}</p>
          <div class="gradient top-0"></div>

          <div class="py-3" v-if="!slug.includes('design-patterns')">
            <Button
              v-if="!activateDownload"
              :custom-style="{ backgroundColor: color, color: '#fff' }"
              size="large"
              class="py-2 my-2 text-xl font-bold text-white"
              :link="link"
              type="link"
              target="_blank"
            >
              {{ isPremium ? 'Grab Your Copy Now' : 'Read Online' }}
            </Button>

            <Button
              v-if="linkToPay && !isWaiting && !isPremium"
              :custom-style="{
                border: `1px solid ${color}`,
                color: `${color} !important`,
              }"
              size="large"
              type="link"
              class="py-2 my-2 text-xl font-bold"
              :link="linkToPay"
            >
              Download Now
            </Button>
          </div>
        </div>

        <!-- Right Image / Form -->
        <div class="w-full md:w-5/12 hero__image">
          <img
            v-if="!isWaiting"
            class="w-full h-auto"
            :src="bookCover"
            :alt="title"
          />

          <div v-else class="shadow border p-4 my-4 bg-white">
            <ResourceForm
              :tag="enchargeTag"
              :color="color"
              :isWaiting="activateDownload"
              :slug="slug"
            />
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="w-full bg-[#f3f6fa]">
    <div class="py-12 container mx-auto">
      <!-- Section Title -->
      <div class="my-10 text-center">
        <h2 class="text-[50px] leading-[65px] font-extrabold text-gray-900">
          Take a peek inside
        </h2>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-start">
        <!-- Left: Book Cover -->
        <div class="flex justify-center">
          <div class="shadow-lg border rounded-lg p-6 my-4 max-w-md w-full">
            <img
              class="w-full h-auto object-contain rounded"
              :src="animatedBookCover"
              :alt="title"
            />
          </div>
        </div>

        <!-- Right: Content -->
        <div>
          <h3
            class="text-[40px] leading-[58.5px] font-bold text-[#121127] py-4"
          >
            {{ title }}
          </h3>

          <p class="text-[#121127]">
            {{ content }}
          </p>

          <hr class="my-6 border-gray-300" />

          <!-- Chapters -->
          <h3
            v-if="chapters?.length"
            class="my-4 text-xl font-semibold text-gray-900"
          >
            What to expect?
          </h3>

          <ul class="space-y-3">
            <li
              v-for="(outline, i) in chapters"
              :key="i"
              class="flex items-center gap-3 text-lg text-[#121127]"
            >
              <!-- Play Icon -->
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                :fill="color"
                width="24"
                height="24"
              >
                <path fill="none" d="M0 0h24v24H0z" />
                <path d="M16 12l-6 6V6z" />
              </svg>

              <span>{{ outline.title }}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed } from 'vue'

// Props
const props = defineProps({
  hub: { type: Object, default: () => ({}) },
})

// Computed
const color = computed(() => {
  const mixedColor = props.hub?.color ?? '#FF715D'
  if (mixedColor.includes('gradient')) {
    const first = mixedColor.split('#')[1]
    const color = first.split(' ')[0]
    return `#${color.replace(/[^a-zA-Z0-9 ]/g, '')}`
  }
  return mixedColor
})

const isPremium = computed(() => props.hub?.is_premium)
const linkToPay = computed(() => props.hub?.link ?? '')
const slug = computed(() => props.hub?.slug ?? '#')
const activateDownload = computed(() => props.hub?.activate_waiting ?? false)
const enchargeTag = computed(() => props.hub?.encharge_tag ?? '')
const title = computed(() => props.hub?.title ?? '')
const description = computed(() => props.hub?.description ?? '')
const content = computed(() => props.hub?.content ?? props.hub?.description)
const chapters = computed(() => {
  const chapters = props.hub?.chapters
  if (!chapters || chapters.length === 0) return []
  if (chapters.length === 1 && chapters[0].posts) return chapters[0].posts
  return chapters
})

const bookCover = computed(
  () =>
    props.hub?.design_url ??
    'https://files.selar.co/product-images/2023/products/masteringbackend/technical-writing-the-com-selar.co-64999ea31aafb.png'
)
const animatedBookCover = computed(
  () =>
    props.hub?.image?.url ??
    'https://res.cloudinary.com/kaperskydisk/image/upload/v1685032766/masteringbackend/posts/vectors/laravel-design-color.png'
)

const link = computed(() =>
  isPremium.value ? linkToPay.value : `/${slug.value}`
)
</script>

<style scoped>
.hero__image img {
  width: 100%;
  height: auto;
}
</style>
