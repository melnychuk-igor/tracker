<script setup>
import { ref, inject } from 'vue'
import { ArrowPathIcon, PauseIcon, PlayIcon } from '@heroicons/vue/24/outline'
import {
  BUTTON_TYPE_SUCCESS,
  BUTTON_TYPE_WARNING,
  BUTTON_TYPE_DANGER,
  MILLISECONDS_IN_SECOND
} from '../constants'
import { currentHour, formatSeconds } from '../functions'
import { isNumber, isTimelineItemValid } from '../validators'
import { updateTimelineItemActivitySecondsKey } from '../keys'
import BaseButton from './BaseButton.vue'

const props = defineProps({
  timelineItem: {
    required: true,
    type: Object,
    validator: isTimelineItemValid
  }
})

const updateTimelineItemActivitySeconds = inject(updateTimelineItemActivitySecondsKey)

const emit = defineEmits({
  updateSeconds: isNumber
})

const seconds = ref(props.timelineItem.activitySeconds)
const isRunning = ref(false)

const isStartButtonDisabled = props.timelineItem.hour !== currentHour()

function start() {
  isRunning.value = true
  isRunning.value = setInterval(() => {
    // emit('updateSeconds', 1)
    updateTimelineItemActivitySeconds(props.timelineItem, 1)
    seconds.value++
  }, MILLISECONDS_IN_SECOND)
}

function stop() {
  clearInterval(isRunning.value)
  isRunning.value = false
}

function reset() {
  stop()
  // emit('updateSeconds', -seconds.value)
  updateTimelineItemActivitySeconds(props.timelineItem, -seconds.value)
  seconds.value = 0
}
</script>

<template>
  <div class="flex w-full gap-2">
    <BaseButton :type="BUTTON_TYPE_DANGER" :disabled="!seconds" @click="reset">
      <ArrowPathIcon class="h-8" />
    </BaseButton>
    <div class="flex flex-grow items-center rounded bg-gray-100 px-2 font-mono text-3xl">
      {{ formatSeconds(seconds) }}
    </div>
    <BaseButton v-if="isRunning" :type="BUTTON_TYPE_WARNING" @click="stop">
      <PauseIcon class="h-8" />
    </BaseButton>
    <BaseButton v-else :type="BUTTON_TYPE_SUCCESS" :disabled="isStartButtonDisabled" @click="start">
      <PlayIcon class="h-8" />
    </BaseButton>
  </div>
</template>
