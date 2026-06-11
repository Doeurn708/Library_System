<template>
  <div >
    <AppNavBar/>
        <main>
          <RouterView/>
        </main>
  </div>
</template>

<script setup>
import AppNavBar from './components/AppNavBar.vue';
</script>
