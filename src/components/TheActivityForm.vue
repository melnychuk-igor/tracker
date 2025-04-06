<script setup>
import { ref, nextTick, inject } from 'vue'
import { PlusIcon } from '@heroicons/vue/24/outline'
import { id } from '../functions'
import { createActivityKey } from '../keys'
import BaseButton from './BaseButton.vue'

const name = ref('')

async function submit() {
  createActivity({
    id: id(),
    name: name.value,
    secondsToComplete: 0
  })
  name.value = ''

  await nextTick()

  window.scrollTo(0, document.body.scrollHeight)
}

const createActivity = inject(createActivityKey)
</script>

<template>
  <form class="sticky bottom-[57px] flex gap-2 border-t bg-white p-4" @submit.prevent="submit">
    <input
      class="w-full rounded border px-4 text-xl"
      type="text"
      placeholder="Activity name"
      v-model="name"
    />
    <BaseButton :disabled="name.trim() === ''">
      <PlusIcon class="h-8" />
    </BaseButton>
  </form>
</template>
