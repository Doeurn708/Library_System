<template>
  <section class="home-hero">
    <div class="hero-copy">
      <span class="eyebrow">Library Management</span>
      <h1>The Bookshop</h1>
      <p>
        Browse the catalog, check copy availability, and open each book record
        from one simple Vue dashboard.
      </p>
      <div class="hero-actions">
        <RouterLink :to="{ name: 'books' }" class="btn btn-warning fw-semibold">
          <i class="fa-solid fa-book-open"></i>
          Browse Books
        </RouterLink>
        <RouterLink to="/about" class="btn btn-outline-dark">
          <i class="fa-solid fa-circle-info"></i>
          About
        </RouterLink>
      </div>
    </div>

    <div class="hero-cover">
      <img :src="featuredBook.cover" :alt="`${featuredBook.title} cover`">
      <div>
        <span>Featured</span>
        <strong>{{ featuredBook.title }}</strong>
        <small>{{ featuredBook.author }}</small>
      </div>
    </div>
  </section>

  <section class="stats-row">
    <div v-for="item in stats" :key="item.label" class="stat-item">
      <strong>{{ item.value }}</strong>
      <span>{{ item.label }}</span>
    </div>
  </section>

  <section class="section-head">
    <div>
      <h2>Recently Added</h2>
      <p>Quick access to active book records.</p>
    </div>
    <RouterLink :to="{ name: 'books' }" class="link-dark fw-semibold">
      View all
    </RouterLink>
  </section>

  <section class="book-strip">
    <RouterLink
      v-for="book in recentBooks"
      :key="book.id"
      :to="{ name: 'book-detail', params: { id: book.id } }"
      class="mini-book"
    >
      <img :src="book.cover" :alt="`${book.title} cover`">
      <span>{{ book.category }}</span>
      <strong>{{ book.title }}</strong>
    </RouterLink>
  </section>
</template>

<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'
import store from '@/data/store.json'

const books = store.books
const featuredBook = books[0]
const recentBooks = computed(() => books.slice(0, 4))
const availableCount = computed(() => books.filter((book) => book.copies > 0).length)
const categoryCount = computed(() => new Set(books.map((book) => book.category)).size)

const stats = computed(() => [
  { label: 'Books', value: books.length },
  { label: 'Available', value: availableCount.value },
  { label: 'Categories', value: categoryCount.value }
])
</script>
