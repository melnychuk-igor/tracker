<script setup>
import { ref } from 'vue'
import TheHeader from './components/TheHeader.vue'
import TheNav from './components/TheNav.vue'
import TheTimeline from './page/TheTimeline.vue'
import TheProgress from './page/TheProgress.vue'
import TheActivities from './page/TheActivities.vue'
import { ClockIcon, ListBulletIcon, ChartBarIcon } from '@heroicons/vue/24/outline'
import { PAGE_TIMELINE, PAGE_ACTIVITIES, PAGE_PROGRESS } from './constants'

const currentPage = ref(normalizePageHash())

function normalizePageHash() {
  const hash = window.location.hash.slice(1)

  if (Object.keys([PAGE_TIMELINE, PAGE_ACTIVITIES, PAGE_PROGRESS]).includes(hash)) {
    return hash
  }

  window.location.hash = PAGE_TIMELINE

  return PAGE_TIMELINE
}

function goTo(page) {
  currentPage.value = page
}
</script>

<template>
  <TheHeader
    @go-to-timeline="currentPage === PAGE_TIMELINE"
    @go-to-progress="currentPage === PAGE_PROGRESS"
  />

  <main class="flex flex-grow">
    <TheTimeline v-show="currentPage === PAGE_TIMELINE" />
    <TheProgress v-show="currentPage === PAGE_PROGRESS" />
    <TheActivities v-show="currentPage === PAGE_ACTIVITIES" />
  </main>

  <TheNav :current-page="currentPage" @navigate="currentPage = $event" />
</template>

<style scoped></style>
