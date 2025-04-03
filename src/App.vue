<script setup>
import { ref, computed, provide } from 'vue'
import TheHeader from './components/TheHeader.vue'
import TheNav from './components/TheNav.vue'
import TheTimeline from './page/TheTimeline.vue'
import TheProgress from './page/TheProgress.vue'
import TheActivities from './page/TheActivities.vue'
import { PAGE_TIMELINE, PAGE_ACTIVITIES, PAGE_PROGRESS, NULLABLE_ACTIVITY } from './constants'
import {
  normalizePageHash,
  generateTimelineItems,
  generateActivities,
  generateActivitySelectOptions
} from './functions'

const currentPage = ref(normalizePageHash())
const activities = ref(generateActivities())
const timelineItems = ref(generateTimelineItems(activities.value))
const activitySelectOptions = computed(() => generateActivitySelectOptions(activities.value))
const timeline = ref()

function goTo(page) {
  if (currentPage.value === PAGE_TIMELINE && page === PAGE_TIMELINE) {
    timeline.value.scrollToHour()
  }

  if (page !== PAGE_TIMELINE) {
    document.body.scrollIntoView()
  }

  currentPage.value = page
}

function createActivity(activity) {
  activities.value.push(activity)
}

function deleteActivity(activity) {
  timelineItems.value.forEach((timelineItem) => {
    if (timelineItem.activityId === activity.id) {
      timelineItem.activityId = null
      timelineItem.activitySeconds = 0
    }
  })

  activities.value.splice(activities.value.indexOf(activity), 1)
}

function setTimelineItemActivity(timelineItem, activity) {
  timelineItem.activityId = activity.id
}

function updateTimelineItemActivitySeconds(timelineItem, activitySeconds) {
  timelineItem.activitySeconds += activitySeconds
  console.log(timelineItem.activitySeconds)
}

function setActivitySecondsToComplete(activity, secondsToComplete) {
  activity.secondsToComplete = secondsToComplete
}

provide('updateTimelineItemActivitySeconds', updateTimelineItemActivitySeconds)
provide('timelineItems', timelineItems.value)
</script>

<template>
  <TheHeader @navigate="goTo($event)" />
  <main class="flex flex-grow flex-col">
    <TheTimeline
      v-show="currentPage === PAGE_TIMELINE"
      :timeline-items="timelineItems"
      :activities="activities"
      :activity-select-options="activitySelectOptions"
      :current-page="currentPage"
      ref="timeline"
      @set-timeline-item-activity="setTimelineItemActivity"
    />
    <TheProgress v-show="currentPage === PAGE_PROGRESS" />
    <TheActivities
      v-show="currentPage === PAGE_ACTIVITIES"
      :activities="activities"
      @create-activity="createActivity"
      @delete-activity="deleteActivity"
      @set-activity-seconds-to-complete="setActivitySecondsToComplete"
    />
  </main>

  <TheNav :current-page="currentPage" @navigate="goTo($event)" />
</template>

<style scoped></style>
