<script setup>
import { ref } from 'vue'
import { PAGE_TIMELINE, PAGE_ACTIVITIES, PAGE_PROGRESS } from '../constants'
import { ClockIcon, ListBulletIcon, ChartBarIcon } from '@heroicons/vue/24/outline'
import NavItem from './NavItem.vue'

defineProps(['currentPage'])

const emit = defineEmits(['navigate'])

const navItems = {
  [PAGE_TIMELINE]: ClockIcon,
  [PAGE_PROGRESS]: ChartBarIcon,
  [PAGE_ACTIVITIES]: ListBulletIcon
}
</script>

<template>
  <nav class="sticky bottom-0 z-10 bg-white text-3xl">
    <ul class="flex items-center justify-around border-t">
      <NavItem
        v-for="(icon, page) in navItems"
        :key="page"
        :href="`#${page}`"
        :class="{ 'pointer-events-none bg-gray-200': page === currentPage }"
        @click="emit('navigate', page)"
      >
        <component :is="icon" class="h-6 w-6" /> {{ page }}
      </NavItem>
    </ul>
  </nav>
</template>

<style lang=""></style>
