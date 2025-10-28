<template>
  <HubsOverview
    title="Backend Engineering Courses"
    desc="Whether you're new to backend engineering, or want to scale up, this is your home for backend engineering courses."
    :hubs="courses"
    :featured="featured"
  />
</template>

<script setup>
import { useHead } from 'nuxt/app'
import { useHubsStore } from '../stores/hubs'

const route = useRoute()
const store = useHubsStore()

definePageMeta({
  layout: 'hub',
})

const { data: courses } = await useAsyncData('courses', async () => {
  let courses = store.courses
  if (!courses.length) {
    const data = {}
    data.page = route.query.page ? route.query.page : 1
    data.count = 22
    courses = await store.getCourses({
      ...data,
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

  return courses
})

const { data: featured } = await useAsyncData('featured', async () => {
  try {
    const res = await store.fetchAllCourses()
    return res?.courses
  } catch (error) {}
})

useHead({
  title: 'Best Backend Engineering Courses',
  meta: [
    {
      hid: 'description',
      name: 'description',
      content: `Access to a catalog of backend courses, backend development courses, advanced backend engineering courses, nestjs courses and backend web development courses. Next-level Backend Engineering courses and Exclusive resources.`,
    },
    {
      hid: 'og:title',
      property: 'og:title',
      content: `Access to a catalog of backend courses, backend development courses, advanced backend engineering courses, nestjs courses and backend web development courses. Next-level Backend Engineering courses and Exclusive resources.`,
    },
    {
      hid: 'og:description',
      property: 'og:description',
      content: `Access to a catalog of backend courses, backend development courses, advanced backend engineering courses, nestjs courses and backend web development courses. Next-level Backend Engineering courses and Exclusive resources.`,
    },
    {
      hid: 'twitter:card',
      name: 'twitter:card',
      content: 'summary_large_image',
    },
  ],
})
</script>

<style scoped></style>