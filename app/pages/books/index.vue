<template>
  <HubsOverview
    title="Welcome To Our Library"
    desc="Whether you're new to backend engineering, or want to scale up, this is your home for backend engineering books."
    :hubs="books"
  />
</template>

<script setup>
import { useRoute } from 'vue-router'
import { useHubsStore } from '~/stores/hubs'

definePageMeta({
  layout: 'hub',
})

const route = useRoute()
const hubsStore = useHubsStore()

const { data: books } = await useAsyncData(`books`, async () => {
  let hubs = hubsStore.getAllBooks()

  console.log(hubs)

  if (!hubs.length) {
    hubs = await hubsStore.getBooks({
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

  return hubs
})

useHead({
  title: 'Backend Engineering Books',
  meta: [
    {
      name: 'description',
      content: `Access to a catalog of backend books, backend development books, advanced backend engineering books, nestjs books and backend web development books. Next-level Backend Engineering books and Exclusive resources.`,
    },
    {
      property: 'og:title',
      content: `Access to a catalog of backend books, backend development books, advanced backend engineering books, nestjs books and backend web development books. Next-level Backend Engineering books and Exclusive resources.`,
    },
    {
      property: 'og:description',
      content: `Access to a catalog of backend books, backend development books, advanced backend engineering books, nestjs books and backend web development books. Next-level Backend Engineering books and Exclusive resources.`,
    },
    {
      name: 'twitter:card',
      content: 'summary_large_image',
    },
  ],
})
</script>
