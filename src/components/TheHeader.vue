<script setup>
// import { CheckCircleIcon } from '@heroicons/vue/24/solid'
import TheLogo from './TheLogo.vue'
import TheHeaderProgress from './TheHeaderProgress.vue'

const emit = defineEmits(['goToTimeline', 'goToProgress'])
</script>

<template>
  <header class="sticky top-0 z-20 flex items-center justify-between border-b bg-white p-3">
    <TheLogo @click="emit('goToTimeline')" />
    <TheHeaderProgress @click="emit('goToProgress')" />
  </header>
</template>

<style lang=""></style>
