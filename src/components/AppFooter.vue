<template>
  <footer class="app-footer">
    <div class="footer-inner">
      <div class="footer-brand">
        <h2>The Bookshop</h2>
        <p>Simple catalog tools for browsing books, checking availability, and managing library records.</p>
      </div>

      <nav class="footer-links" aria-label="Footer navigation">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink :to="{ name: 'books' }">Books</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/login">Account</RouterLink>
      </nav>

      <div class="footer-contact">
        <span><i class="fa-solid fa-location-dot"></i> Library Desk</span>
        <span><i class="fa-solid fa-envelope"></i> support@thebookshop.local</span>
      </div>
    </div>

    <div class="footer-bottom">
      <span>&copy; {{ year }} The Bookshop. All rights reserved.</span>
      <span>Library Management System</span>
    </div>
  </footer>
</template>

<script setup>
import { RouterLink } from 'vue-router'

const year = new Date().getFullYear()
</script>
