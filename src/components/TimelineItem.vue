<script setup>
import { ref } from 'vue'
import { isTimelineItemValid, validateSelectOptions } from '../validators'
import BaseSelect from './BaseSelect.vue'
import TimelineHour from './TimelineHour.vue'

const props = defineProps({
  timelineItem: {
    required: true,
    type: Object,
    validator: isTimelineItemValid
  },
  activitySelectOptions: {
    required: true,
    type: Array
    // validator: validateSelectOptions
  }
})

const selectedActivityId = ref(3)
</script>

<template>
  <li class="relative flex flex-col gap-2 border-t border-gray-200 px-4 py-10">
    <TimelineHour :hour="timelineItem.hour" />
  </li>

  <BaseSelect
    :selected="selectedActivityId"
    :options="activitySelectOptions"
    placeholder="Rest"
    @select="selectedActivityId = $event"
  />
</template>

<style lang=""></style>
