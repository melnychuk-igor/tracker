<script setup>
import { ref } from 'vue'
import { TrashIcon } from '@heroicons/vue/24/outline'
import BaseButton from './BaseButton.vue'
import BaseSelect from './BaseSelect.vue'
import { PERIOD_SELECT_OPTIONS } from '../constants'
import { isActivityValid, isUndefined } from '../validators'
import { BUTTON_TYPE_DANGER, BUTTON_TYPE_NEUTRAL } from '../constants'

defineProps({
  activity: {
    required: true,
    type: String,
    validator: isActivityValid
  }
})

const emit = defineEmits({
  delete: isUndefined
})

const secondsToComplete = ref(null)
</script>

<template>
  <li class="flex flex-col gap-2 p-4">
    <div class="flex items-center gap-2">
      <BaseButton :type="BUTTON_TYPE_DANGER" @click="emit('delete')">
        <TrashIcon class="h-8" />
      </BaseButton>
      <span class="truncate text-xl">{{ activity }}</span>
    </div>
    <div>
      <BaseSelect
        class="font-mono"
        placeholder="h:mm"
        :options="PERIOD_SELECT_OPTIONS"
        :selected="secondsToComplete"
        @select="secondsToComplete = $event"
      />
    </div>
  </li>
</template>

<style lang=""></style>
