<script setup lang="ts">
import TheHeaderProgress from './TheHeaderProgress.vue'
import TheLogo from './TheLogo.vue'
import TheContactLink from './TheContactLink.vue'
</script>

<template>
  <header
    class="sticky top-0 z-20 flex items-center justify-between border-b bg-white p-3 md:max-xl:w-3 lg:w-3"
  >
    <div class="flex items-center gap-4">
      <TheLogo />
      <TheContactLink />
    </div>
    <TheHeaderProgress />
  </header>
</template>
