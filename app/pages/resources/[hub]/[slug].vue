<!-- eslint-disable vue/no-v-html -->
<template>
  <main class="hero__grid">
    <ResourceOverview :post="post" :chapter="chapter" />
  </main>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { usePostStore } from '~/stores/post'
import { useHubsStore } from '~/stores/hubs'

// composables
const route = useRoute()
const postStore = usePostStore()
const hubsStore = useHubsStore()

// Fetch post and chapter with Pinia
const { data: post } = await useAsyncData(
  `post-${route.params.slug}`,
  async () => {
    let post = await postStore.getPostBySlug(route.params.slug)
    if (!post) {
      post = await postStore.getPost({
        slug: route.params.slug,
        populate: 'chapter',
      })
    }
    return post
  }
)

const { data: chapter } = await useAsyncData(
  `chapter-${route.params.slug}`,
  async () => {
    const slug = post.value?.chapter?.slug ?? route.params.slug
    let chapter = await hubsStore.getChapterBySlug(slug)

    if (!chapter) {
      chapter = await hubsStore.getChapter({ slug })
    }
    return chapter
  }
)

// constants
const BASE_URL =
  useRuntimeConfig().public.baseUrl || 'https://masteringbackend.com'

// computed fallback image
const image = computed(() => {
  if (!post.value) return '/img/backend-2023.jpg'
  return (
    post.value?.image ||
    post.value?.featured_image?.url ||
    '/img/backend-2023.jpg'
  )
})

// helpers
const stripTags = (text) => (text ? text.replace(/(<([^>]+)>)/gi, '') : '')
const splitTags = (tags) =>
  Array.isArray(tags) ? tags.map((tag) => tag.title).join(', ') : ''

// SEO
useHead(() => {
  const current = post.value || chapter.value
  if (!current) return {}

  return {
    title: current.title,
    meta: [
      {
        hid: 'keywords',
        name: 'keywords',
        content: splitTags(current?.tags) ?? '',
      },
      {
        hid: 'description',
        name: 'description',
        content: stripTags(current?.description || current?.excerpt),
      },
      { hid: 'og:title', property: 'og:title', content: current.title },
      {
        hid: 'og:description',
        property: 'og:description',
        content: stripTags(current?.description || current?.excerpt),
      },
      { hid: 'og:image', property: 'og:image', content: image.value },
      {
        hid: 'og:url',
        property: 'og:url',
        content: `${BASE_URL}/resources/${route.params.hub}/${route.params.slug}`,
      },
      { hid: 'og:image:width', property: 'og:image:width', content: '800' },
      { hid: 'og:image:height', property: 'og:image:height', content: '800' },
      { hid: 'og:type', property: 'og:type', content: 'article' },
      {
        hid: 'article:published_time',
        property: 'article:published_time',
        content: current?.createdAt,
      },
      {
        hid: 'article:modified_time',
        property: 'article:modified_time',
        content: current?.updatedAt,
      },
      {
        hid: 'twitter:card',
        name: 'twitter:card',
        content: 'summary_large_image',
      },
    ],
  }
})
</script>

<style scoped>
</style>
