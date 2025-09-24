<template>
  <main class="hero__grid">
    <BookOverview v-if="hub" :hub="hub" />
  </main>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { useHubsStore } from '~/stores/hubs'

const route = useRoute()
const hubsStore = useHubsStore()
const BASE_URL = process.env.BASE_URL || 'https://masteringbackend.com'

// fetch hub data
const { data: hub } = await useAsyncData(
  `book-${route.params.slug}`,
  async () => {
    let hub = await hubsStore.getHubBySlug(route.params.slug)
    if (hub) return hub

    return await hubsStore.getHub({
      slug: route.params.slug,
      populate: {
        image: true,
        chapters: {
          populate: {
            posts: true,
          },
        },
      },
    })
  }
)

// helpers
const stripTags = (text) => (text ? text.replace(/(<([^>]+)>)/gi, '') : '')
const splitTags = (tags) =>
  Array.isArray(tags) ? tags.map((tag) => tag.title).join(', ') : ''

const image = computed(
  () => hub.value?.design_url ?? hub.value?.image ?? '/img/backend-2023.jpg'
)

// SEO
useHead(() => {
  if (!hub.value) return {}

  return {
    title: hub.value.title,
    meta: [
      {
        name: 'keywords',
        content: hub.value.chapters?.map((c) => c.title).join(', '),
      },
      {
        name: 'description',
        content: stripTags(hub.value.description),
      },
      { property: 'og:image', content: image.value },
      { property: 'og:title', content: hub.value.title },
      {
        property: 'og:description',
        content: stripTags(hub.value.description),
      },
      { property: 'og:url', content: `${BASE_URL}/books/${route.params.slug}` },
      { property: 'og:image:width', content: '800' },
      { property: 'og:image:height', content: '800' },
      { property: 'og:type', content: 'article' },
      {
        property: 'article:published_time',
        content: hub.value.createdAt,
      },
      {
        property: 'article:modified_time',
        content: hub.value.updatedAt,
      },
      { name: 'twitter:card', content: 'summary_large_image' },
    ],
  }
})
</script>

<style scoped>
</style>
