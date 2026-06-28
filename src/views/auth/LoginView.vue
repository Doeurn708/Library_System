<template>
  <div class="login-wrap">
    <div class="login-card">

      <!-- Header -->
      <div class="brand">
        <div class="brand-icon">
          <i class="fa-solid fa-book-open"></i>
        </div>
        <h2>Welcome back</h2>
        <p>Sign in to your bookshop account</p>
      </div>

      <!-- Success state -->
      <div v-if="success" class="alert alert-success d-flex align-items-center gap-2 mb-3">
        <i class="fa-solid fa-circle-check"></i>
        Signed in successfully! Redirecting...
      </div>

      <form novalidate @submit.prevent="handleSubmit">

        <!-- Email -->
        <div class="mb-3">
          <label for="email" class="form-label">Email address</label>
          <div class="input-group">
            <span class="input-group-text">
             <i class="fa-regular fa-envelope"></i>
            </span>
            <input
              id="email"
              v-model.trim="form.email"
              type="email"
              class="form-control"
              :class="{ 'is-invalid': errors.email, 'is-valid': touched.email && !errors.email }"
              placeholder="@example.com"
              autocomplete="email"
              @blur="touch('email')"
            />
            <div class="invalid-feedback">{{ errors.email }}</div>
          </div>
        </div>

        <!-- Password -->
        <div class="mb-3">
          <label for="password" class="form-label">Password</label>
          <div class="input-group">
            <span class="input-group-text">
              <i class="fa-solid fa-key"></i>
            </span>
            <input
              id="password"
              v-model="form.password"
              :type="showPassword ? 'text' : 'password'"
              class="form-control"
              :class="{ 'is-invalid': errors.password, 'is-valid': touched.password && !errors.password }"
              placeholder=" password 6 characters"
              autocomplete="current-password"
              @blur="touch('password')"
            />
            <button
              type="button"
              class="input-group-text btn-toggle-pw"
              :aria-label="showPassword ? 'Hide password' : 'Show password'"
              @click="showPassword = !showPassword"
            >
              <i :class="showPassword ? 'fa-regular fa-eye' : 'fa-regular fa-eye-slash'"></i>
            </button>
            <div class="invalid-feedback">{{ errors.password }}</div>
          </div>
        </div>

        <!-- Remember + Forgot -->
        <div class="d-flex justify-content-between align-items-center mb-4">
          <div class="form-check">
            <input
              id="remember"
              v-model="form.remember"
              type="checkbox"
              class="form-check-input"
            />
            <label class="form-check-label text-muted" for="remember">Remember me</label>
          </div>
          <RouterLink to="/forgot-password" class="link-accent text-decoration-none">
            Forgot password?
          </RouterLink>
        </div>

        <!-- Submit -->
        <button
          type="submit"
          class="btn btn-primary w-100 fw-semibold mb-3"
          :disabled="loading"
        >
          <span v-if="loading" class="spinner-border spinner-border-sm me-2" role="status" aria-hidden="true"></span>
          {{ loading ? 'Signing in...' : 'Sign In' }}
        </button>

        <!-- Sign up -->
        <p class="text-center text-muted mb-0" style="font-size: 0.875rem;">
          Don't have an account?
          <RouterLink to="/register" class="link-accent text-decoration-none fw-medium">
            Create one
          </RouterLink>
        </p>

      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const form = reactive({ email: '', password: '', remember: false })
const touched = reactive({ email: false, password: false })
const loading = ref(false)
const success = ref(false)
const showPassword = ref(false)

const errors = computed(() => {
  const e = {}
  if (touched.email) {
    if (!form.email) e.email = 'Email is required.'
    else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) e.email = 'Please enter a valid email.'
  }
  if (touched.password) {
    if (!form.password) e.password = 'Password is required.'
    else if (form.password.length < 6) e.password = 'Password must be at least 6 characters.'
  }
  return e
})

function touch(field) {
  touched[field] = true
}

async function handleSubmit() {
  touched.email = true
  touched.password = true
  if (errors.value.email || errors.value.password) return

  loading.value = true
  try {
    // await authStore.login(form.email, form.password, form.remember)
    await new Promise(r => setTimeout(r, 1200)) // replace with real API call
    success.value = true
    setTimeout(() => router.push('/'), 1000)
  } catch (err) {
    console.error(err)
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.login-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #f8f5f0;
}

.login-card {
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 2px 24px rgba(0, 0, 0, 0.08);
  padding: 2.5rem 2rem;
  width: 100%;
  max-width: 400px;
}

.brand {
  text-align: center;
  margin-bottom: 2rem;
}

.brand-icon {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.brand h2 {
  font-family: Georgia, serif;
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.25rem;
}

.brand p {
  font-size: 0.875rem;
  color: #6c757d;
  margin: 0;
}

.input-group-text {
  background: #f8f5f0;
  border-color: #dee2e6;
  color: #6c757d;
}

.btn-toggle-pw {
  cursor: pointer;
  border-left: 0;
}

.btn-primary {
  background-color: #c4853a;
  border-color: #c4853a;
  padding: 0.65rem;
}

.btn-primary:hover,
.btn-primary:focus {
  background-color: #b07230;
  border-color: #b07230;
}

.btn-primary:disabled {
  background-color: #c4853a;
  border-color: #c4853a;
  opacity: 0.75;
}

.form-check-input:checked {
  background-color: #c4853a;
  border-color: #c4853a;
}

.link-accent {
  color: #c4853a;
}

.link-accent:hover {
  color: #b07230;
  text-decoration: underline !important;
}
</style>
