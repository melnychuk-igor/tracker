<script setup lang="ts">
import BaseIcon from './BaseIcon.vue'
import { navigate } from '../router'
import { IconName, PageName } from '../types'

function handleClick(): void {
  navigate(PageName.CONTACT)
}
</script>

<template>
  <a class="block h-auto w-[32px]" :href="`#${PageName.CONTACT}`" @click="handleClick">
    <BaseIcon :name="IconName.CHAT" />
  </a>
</template>
