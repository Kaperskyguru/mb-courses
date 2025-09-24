<template>
  <HubsOverview
    title="Welcome To Our Resources Page"
    desc="Whether you're new to backend engineering, or want to scale up, this is your home for Exclusive backend engineering resources."
    :hubs="hubs"
  />
</template>

<script setup>
import { useHubsStore } from '~/stores/hubs'

const route = useRoute()
const store = useHubsStore()

// Async data fetching
const { data: hubs } = await useAsyncData('hubs-resources', async () => {
  try {
    let hubs = store.getAllPDFs

    if (!hubs?.length) {
      const data = {
        page: route.query.page ? route.query.page : 1,
        count: 22,
        populate: {
          image: true,
          chapters: {
            populate: {
              posts: true,
            },
          },
        },
      }
      hubs = await store.getPDFs(data)
    }
    return hubs
  } catch (error) {
    return []
  }
})

useHead({
  title: 'Resources',
  meta: [
    {
      hid: 'description',
      name: 'description',
      content:
        'A great resource for backend engineers. Next-level Backend Engineering training and Exclusive resources.',
    },
    {
      hid: 'og:title',
      property: 'og:title',
      content:
        'A great resource for backend engineers. Next-level Backend Engineering training and Exclusive resources.',
    },
    {
      hid: 'og:description',
      property: 'og:description',
      content:
        'A great resource for backend engineers. Next-level Backend Engineering training and Exclusive resources.',
    },
    {
      hid: 'twitter:card',
      name: 'twitter:card',
      content: 'summary_large_image',
    },
  ],
})
</script>

<style scoped>
</style>
