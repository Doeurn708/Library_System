<template>
  <section class="about-page">
    <div class="about-copy">
      <span class="eyebrow">About</span>
      <h1>Built for fast library catalog work.</h1>
      <p>
        This Vue library management system keeps book records easy to scan,
        search, and open. It uses local catalog data for a simple front-end
        workflow that can later connect to an API.
      </p>
    </div>

    <div class="about-panel">
      <div v-for="item in metrics" :key="item.label" class="about-metric">
        <strong>{{ item.value }}</strong>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </section>

  <section class="feature-list">
    <article v-for="feature in features" :key="feature.title">
      <i :class="feature.icon"></i>
      <h2>{{ feature.title }}</h2>
      <p>{{ feature.text }}</p>
    </article>
  </section>
</template>

<script setup>
import { computed } from 'vue'
import store from '@/data/store.json'

const books = store.books

const metrics = computed(() => [
  { label: 'Catalog Books', value: books.length },
  { label: 'Available Copies', value: books.reduce((sum, book) => sum + book.copies, 0) },
  { label: 'Categories', value: new Set(books.map((book) => book.category)).size }
])

const features = [
  {
    icon: 'fa-solid fa-magnifying-glass',
    title: 'Searchable Catalog',
    text: 'Find books by title, author, category, or ISBN from the Books page.'
  },
  {
    icon: 'fa-solid fa-layer-group',
    title: 'Organized Records',
    text: 'Each detail page shows availability, price, metadata, and description.'
  },
  {
    icon: 'fa-solid fa-user-check',
    title: 'Borrowing Flow',
    text: 'Local borrow and return actions update copy status instantly in the browser.'
  }
]
</script>
