<template>
  <div class="app-shell">
    <AppNavBar />
    <main class="app-content">
      <RouterView />
    </main>
    <AppFooter />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router'
import AppNavBar from './components/AppNavBar.vue'
import AppFooter from './components/AppFooter.vue'
</script>
