<template>
  <section v-if="book" class="book-detail">
    <div class="cover-panel">
      <img :src="book.cover" :alt="`${book.title} cover`">
    </div>

    <div class="detail-panel">
      <div class="d-flex justify-content-between align-items-center">
        <RouterLink :to="{ name: 'books' }" class="btn btn-outline-secondary mb-4">
          <i class="fa-solid fa-arrow-left"></i>
          Back
        </RouterLink>
        <span class="badge bg-danger py-2">{{ book.category }}</span>
      </div>
      <h1>{{ book.title }}</h1>
      <h5 class="text-muted mb-3">By {{ book.author }}</h5>

      <div class="d-flex flex-wrap align-items-center gap-3 mb-4">
        <span class="status-pill" :class="statusClass">{{ book.status }}</span>
        <strong>{{ book.copies }} copies</strong>
        <span class="rating">
          <i class="fa-solid fa-star"></i>
          {{ book.rating }}
        </span>
      </div>

      <h2 class="text-success fw-bold mb-4">${{ book.price.toFixed(2) }}</h2>
      <p class="text-secondary">{{ book.description }}</p>
      <p class="detail-summary">{{ book.summary }}</p>

      <div class="action-row">
        <button class="btn btn-dark" type="button" :disabled="book.copies === 0">
          <i class="fa-solid fa-hand-holding"></i>
          {{ book.copies === 0 ? 'Unavailable' : 'Borrow Book' }}
        </button>
        <button @click="PassActive" class="btn"
        :class="active ? 'btn-warning text-white' : 'btn-outline-warning'"  type="button">
          <i :class="active ? 'fa-solid fa-bookmark' : 'fa-regular fa-bookmark '"></i>
          {{ active ? 'Saved' : 'Save' }}
        </button>
      </div>

      <div class="detail-grid">
        <div>
          <strong>Pages</strong>
          <p>{{ book.pages }}</p>
        </div>
        <div>
          <strong>Language</strong>
          <p>{{ book.language }}</p>
        </div>
        <div>
          <strong>Published</strong>
          <p>{{ book.published }}</p>
        </div>
        <div>
          <strong>Publisher</strong>
          <p>{{ book.publisher }}</p>
        </div>
        <div>
          <strong>Format</strong>
          <p>{{ book.format }}</p>
        </div>
        <div>
          <strong>Shelf</strong>
          <p>{{ book.shelf }}</p>
        </div>
        <div>
          <strong>Borrowed</strong>
          <p>{{ book.borrowedCount }} times</p>
        </div>
        <div>
          <strong>ISBN</strong>
          <p>{{ book.isbn }}</p>
        </div>
      </div>

      <section class="topics-panel">
        <h2>Key Topics</h2>
        <div class="topic-list">
          <span v-for="topic in book.topics" :key="topic">{{ topic }}</span>
        </div>
      </section>
    </div>
  </section>

  <section v-if="book && relatedBooks.length" class="related-section">
    <div class="related-head">
      <h2>Related Books</h2>
      <RouterLink :to="{ name: 'books' }" class="link-dark fw-semibold">View catalog</RouterLink>
    </div>

    <div class="related-grid">
      <RouterLink
        v-for="item in relatedBooks"
        :key="item.id"
        :to="{ name: 'book-detail', params: { id: item.id } }"
        class="related-card"
      >
        <img :src="item.cover" :alt="`${item.title} cover`">
        <div>
          <span>{{ item.category }}</span>
          <strong>{{ item.title }}</strong>
          <small>{{ item.author }}</small>
        </div>
      </RouterLink>
    </div>
  </section>

  <section v-if="!book" class="book-missing">
    <h1>Book not found</h1>
    <RouterLink :to="{ name: 'books' }" class="btn btn-primary">
      Back to Books
    </RouterLink>
  </section>
</template>

<script setup>
import { ref } from 'vue';
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import store from '@/data/store.json'

const active = ref(false );
const PassActive = ()=>{
  active.value= !active.value
  
}

const route = useRoute()

const book = computed(() => {
  const id = Number(route.params.id)
  return store.books.find((item) => item.id === id)
})

const statusClass = computed(() => {
  if (!book.value) return ''

  return book.value.status.toLowerCase()
})

const relatedBooks = computed(() => {
  if (!book.value) return []

  return store.books
    .filter((item) => item.id !== book.value.id && item.category === book.value.category)
    .slice(0, 3)
})
</script>

<style scoped>
.book-detail {
  display: grid;
  grid-template-columns: minmax(280px, 0.8fr) minmax(0, 1.2fr);
  min-height: calc(100vh - 8rem);
  overflow: hidden;
  border: 1px solid #eedcc3;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 12px 30px rgba(88, 62, 34, 0.06);
}

.cover-panel {
  display: grid;
  place-items: center;
  min-height: 520px;
  padding: 2rem;
  background: #f4eadc;
}

.cover-panel img {
  width: min(100%, 340px);
  max-height: 520px;
  object-fit: contain;
  border-radius: 6px;
  box-shadow: 0 18px 35px rgba(31, 41, 51, 0.18);
}

.detail-panel {
  align-self: center;
  padding: 3rem;
}

.detail-panel h1,
.book-missing h1 {
  margin: 0 0 0.75rem;
  color: #17212b;
  font-family: Georgia, "Times New Roman", serif;
}

.detail-panel h1 {
  font-size: clamp(2rem, 4vw, 3.5rem);
}

.rating {
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  color: #a4641e;
  font-weight: 800;
}

.detail-summary {
  padding: 1rem;
  border-left: 4px solid #c4853a;
  border-radius: 6px;
  background: #fffaf3;
  color: #3b4650;
  font-weight: 600;
}

.action-row {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  margin: 1.5rem 0;
}

.detail-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 1rem;
  margin-top: 2rem;
}

.detail-grid div {
  padding: 1rem;
  border: 1px solid #eedcc3;
  border-radius: 8px;
  background: #fffaf3;
}

.detail-grid p {
  margin: 0.35rem 0 0;
  color: #61707f;
}

.topics-panel {
  margin-top: 2rem;
}

.topics-panel h2,
.related-head h2 {
  margin: 0 0 1rem;
  color: #17212b;
  font-family: Georgia, "Times New Roman", serif;
}

.topic-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.topic-list span {
  padding: 0.4rem 0.75rem;
  border: 1px solid #eedcc3;
  border-radius: 999px;
  background: #fffaf3;
  color: #3b4650;
  font-weight: 700;
}

.related-section {
  margin-top: 1.5rem;
}

.related-head {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  align-items: center;
  margin-bottom: 1rem;
}

.related-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
}

.related-card {
  display: grid;
  grid-template-columns: 96px minmax(0, 1fr);
  gap: 1rem;
  align-items: center;
  padding: 1rem;
  border: 1px solid #eedcc3;
  border-radius: 8px;
  background: #fff;
  color: inherit;
  text-decoration: none;
}

.related-card img {
  width: 96px;
  aspect-ratio: 3 / 4;
  object-fit: cover;
  border-radius: 6px;
}

.related-card span,
.related-card small {
  display: block;
  color: #61707f;
}

.related-card strong {
  display: block;
  margin: 0.25rem 0;
  font-family: Georgia, "Times New Roman", serif;
  line-height: 1.15;
}

.book-missing {
  display: grid;
  justify-items: start;
  gap: 1rem;
  padding: 3rem;
  border: 1px solid #eedcc3;
  border-radius: 8px;
  background: #fff;
}

@media (max-width: 767.98px) {
  .book-detail {
    grid-template-columns: 1fr;
  }

  .cover-panel {
    min-height: auto;
  }

  .detail-panel {
    padding: 1.5rem;
  }

  .detail-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .related-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 575.98px) {
  .detail-grid {
    grid-template-columns: 1fr;
  }

  .related-head {
    align-items: start;
    flex-direction: column;
  }
}
</style>
