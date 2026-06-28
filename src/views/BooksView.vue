<template>
  <section class="books-header">
    <div>
      <span class="eyebrow">Catalog</span>
      <h1>Books</h1>
      <p>Search, filter, sort, and manage local copy availability.</p>
    </div>
    <button class="btn btn-outline-secondary" type="button" @click="resetFilters">
      <i class="fa-solid fa-rotate-left"></i>
      Reset
    </button>
  </section>

  <section class="catalog-tools">
    <div class="input-group">
      <span class="input-group-text"><i class="fa-solid fa-magnifying-glass"></i></span>
      <input
        v-model.trim="search"
        type="search"
        class="form-control"
        placeholder="Search title, author, category, or ISBN"
      >
    </div>

    <select v-model="category" class="form-select" aria-label="Filter by category">
      <option value="all">All categories</option>
      <option v-for="item in categories" :key="item" :value="item">{{ item }}</option>
    </select>

    <select v-model="availability" class="form-select" aria-label="Filter by availability">
      <option value="all">All status</option>
      <option value="available">Available</option>
      <option value="borrowed">Borrowed</option>
    </select>

    <select v-model="sortBy" class="form-select" aria-label="Sort books">
      <option value="title">Title</option>
      <option value="author">Author</option>
      <option value="price">Price</option>
      <option value="copies">Copies</option>
    </select>
  </section>

  <section v-if="filteredBooks.length" class="book-grid">
    <article v-for="book in filteredBooks" :key="book.id" class="book-card">
      <RouterLink :to="{ name: 'book-detail', params: { id: book.id } }" class="cover-link">
        <img :src="book.cover" :alt="`${book.title} cover`">
      </RouterLink>

      <div class="book-card-body">
        <div class="d-flex justify-content-between align-items-start gap-2">
          <span class="badge text-bg-warning ">{{ book.category }}</span>
          <span class="status-pill" :class="statusClass(book)">{{ book.status }}</span>
        </div>

        <RouterLink :to="{ name: 'book-detail', params: { id: book.id } }" class="book-title fs-5">
          {{ book.title }}
        </RouterLink>
        <p class="text-muted mb-2 fs-5">By {{ book.author }}</p>
        <p class="book-desc">{{ book.description }}</p>

        <div class="book-meta">
          <span><i class="fa-solid fa-copy"></i> {{ book.copies }} copies</span>
          <span>${{ book.price.toFixed(2) }}</span>
        </div>

        <button
          type="button"
          class="btn w-100"
          :class="book.copies > 0 ? 'btn-dark' : 'btn-outline-secondary'"
          @click="toggleBorrow(book)"
        >
          <i :class="book.copies > 0 ? 'fa-solid fa-hand-holding' : 'fa-solid fa-rotate-left'"></i>
          {{ book.copies > 0 ? 'Borrow Copy' : 'Return Copy' }}
        </button>
      </div>
    </article>
  </section>

  <section v-else class="empty-state">
    <i class="fa-solid fa-book"></i>
    <h2>No books found</h2>
    <p>Try another search term or clear the filters.</p>
  </section>
</template>

<script setup>
import { computed, ref, watch } from 'vue'
import { RouterLink, useRoute, useRouter } from 'vue-router'
import store from '@/data/store.json'

const route = useRoute()
const router = useRouter()

const books = ref(store.books.map((book) => ({ ...book })))
const search = ref(String(route.query.search || ''))
const category = ref('all')
const availability = ref('all')
const sortBy = ref('title')

const categories = computed(() => [...new Set(books.value.map((book) => book.category))].sort())

const filteredBooks = computed(() => {
  const term = search.value.toLowerCase()

  return books.value
    .filter((book) => {
      const matchesSearch = [book.title, book.author, book.category, book.isbn]
        .join(' ')
        .toLowerCase()
        .includes(term)
      const matchesCategory = category.value === 'all' || book.category === category.value
      const matchesAvailability =
        availability.value === 'all' ||
        (availability.value === 'available' && book.copies > 0) ||
        (availability.value === 'borrowed' && book.copies === 0)

      return matchesSearch && matchesCategory && matchesAvailability
    })
    .sort((a, b) => {
      if (sortBy.value === 'price' || sortBy.value === 'copies') {
        return b[sortBy.value] - a[sortBy.value]
      }

      return String(a[sortBy.value]).localeCompare(String(b[sortBy.value]))
    })
})

watch(
  () => route.query.search,
  (value) => {
    search.value = String(value || '')
  }
)

function resetFilters() {
  search.value = ''
  category.value = 'all'
  availability.value = 'all'
  sortBy.value = 'title'
  router.replace({ name: 'books' })
}

function statusClass(book) {
  return book.copies > 0 ? 'available' : 'borrowed'
}

function toggleBorrow(book) {
  if (book.copies > 0) {
    book.copies -= 1
  } else {
    book.copies += 1
  }

  book.status = book.copies > 0 ? 'Available' : 'Borrowed'
}
</script>
