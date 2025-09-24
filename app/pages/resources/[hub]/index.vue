<template>
  <HubOverview :hub="hub" />
</template>

<script setup>
import { useRoute } from 'vue-router'
import { useHubsStore } from '~/stores/hubs'

// composables
const route = useRoute()
const hubsStore = useHubsStore()

// Fetch hub with Pinia
const { data: hub } = await useAsyncData(
  `hub-${route.params.hub}`,
  async () => {
    let hub = await hubsStore.getHubBySlug(route.params.hub)

    if (!hub || !hub.length) {
      hub = await hubsStore.getHub({
        slug: route.params.hub,
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
    return hub
  }
)

// constants
const BASE_URL =
  useRuntimeConfig().public.baseUrl || 'https://masteringbackend.com'

// util helpers
const stripTags = (text) => (text ? text.replace(/(<([^>]+)>)/gi, '') : '')
const splitTags = (tags) =>
  Array.isArray(tags) ? tags.map((tag) => tag.title).join(', ') : ''

// SEO
useHead(() => {
  if (!hub.value) return {}
  return {
    title: hub.value?.title,
    meta: [
      {
        hid: 'keywords',
        name: 'keywords',
        content: splitTags(hub.value?.tags),
      },
      {
        hid: 'description',
        name: 'description',
        content: stripTags(hub.value?.description),
      },
      { hid: 'og:title', property: 'og:title', content: hub.value?.title },
      {
        hid: 'og:description',
        property: 'og:description',
        content: stripTags(hub.value?.description),
      },
      { hid: 'og:image', property: 'og:image', content: hub.value?.image },
      {
        hid: 'og:url',
        property: 'og:url',
        content: `${BASE_URL}/resources/${hub.value?.slug}`,
      },
      { hid: 'og:image:width', property: 'og:image:width', content: '800' },
      { hid: 'og:image:height', property: 'og:image:height', content: '800' },
      { hid: 'og:type', property: 'og:type', content: 'article' },
      {
        hid: 'article:published_time',
        property: 'article:published_time',
        content: hub.value?.createdAt,
      },
      {
        hid: 'article:modified_time',
        property: 'article:modified_time',
        content: hub.value?.updatedAt,
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
