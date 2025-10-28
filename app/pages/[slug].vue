<template>
  <CourseOverview :hub="hub" :course="course" />
</template>
    
    <script setup>
import { useRoute } from 'nuxt/app'
import { useHubsStore } from '../stores/hubs'

const route = useRoute()
const store = useHubsStore()

const { data: hub } = await useAsyncData('hub', async () => {
  // check local store first

  let hub = store.getHubBySlug(route.params.hub)

  if (hub) return hub

  hub = await store.getHub({
    slug: route.params.slug,
    populate: {
      image: true,
      topics: true,
      author: true,
      chapters: {
        populate: { posts: true },
      },
    },
  })

  return hub
})

const { data: course } = await useAsyncData('course', async () => {
  try {
    const course = await store.fetchCourseContent({
      slug: route.params.slug,
    })

    return course
  } catch (error) {}
})

const BASE_URL = ref(process.env.BASE_URL || 'https://masteringbackend.com')

const language = computed(() => {
  const text = hub.value?.title?.split('Become A ')[1]
  return text?.split('Backend Engineer')[0]
})

const image = computed(() => {
  if (this?.hub && hub.value?.design_url) return hub.value?.design_url
  if (this?.hub && hub.value?.image) return hub.value?.image
  return '/img/backend-2023.jpg'
})

function stripTags(text) {
  if (text) {
    return text.replace(/(<([^>]+)>)/gi, '')
  }
}
function splitTags(tags) {
  if (Array.isArray(tags)) {
    return tags.map((tag) => tag.title).join(', ')
  }
}

useHead({
  title: `Advanced ${language.value}Course: ${hub.value.title}`,
  meta: [
    {
      hid: 'keywords',
      name: 'keywords',
      content: `${hub.value?.chapters?.map((chapter) => {
        return chapter.title
      })}`,
    },
    {
      hid: 'description',
      name: 'description',
      content: `Advanced ${
        language.value
      } Course for Backend Engineers: ${stripTags(hub.value?.description)}`,
    },
    { hid: 'og:image', property: 'og:image', content: image.value },
    {
      hid: 'og:title',
      property: 'og:title',
      content: `Advanced ${language.value} Course: ${hub.value.title}`,
    },
    {
      hid: 'og:description',
      property: 'og:description',
      content: `Advanced ${
        language.value
      } Course for Backend Engineers: ${stripTags(hub.value?.description)}`,
    },
    { hid: 'og:image', property: 'og:image', content: image.value },
    {
      hid: 'og:url',
      property: 'og:url',
      content: `${BASE_URL.value}/courses/${route.params.slug}`,
    },
    {
      hid: 'og:image:width',
      property: 'og:image:width',
      content: '800',
    },
    {
      hid: 'og:image:height',
      property: 'og:image:height',
      content: '800',
    },
    {
      hid: 'twitter:card',
      name: 'twitter:card',
      content: 'summary_large_image',
    },
  ],
})
</script>
    
  