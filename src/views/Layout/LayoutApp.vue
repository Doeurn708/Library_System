<template>
  <div class="app-shell">
    <AppNavBar />
    <main class="app-content">
      <RouterView />
    </main>
  </div>
</template>

<script setup>
import AppNavBar from '@/components/AppNavBar.vue'
import { RouterView } from 'vue-router'
</script>
