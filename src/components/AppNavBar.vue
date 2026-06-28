<template>
    <div class="bg-white text-warning p-2 d-flex justify-content-between align-items-center border-bottom border-warning shadow">
        <div @click="OpenMenu" class="fs-4 d-md-none" style="cursor: pointer;">
            <i :class="isOpen ? 'fa-solid fa-xmark' : 'fa-solid fa-bars'"></i>
        </div>

        <div class="px-3 d-none d-md-flex">
            <h1 class="fs-3 fw-bold m-0">The Bookshop</h1>
        </div>

        <div class="d-none font-monospace fw-medium gap-4 d-md-flex fs-5">
            <RouterLink to="/" class="text-decoration-none text-black custom-menu-link">Home</RouterLink>
            <RouterLink :to="{ name: 'books' }" class="text-decoration-none text-black custom-menu-link">Books</RouterLink>
            <RouterLink to="/about" class="text-decoration-none text-black custom-menu-link">About</RouterLink>
        </div>

        <div class="d-flex gap-3 px-4 justify-content-center align-items-center">
            <div class="input-group">
                <div class="input-group-text" id="btnGroupAddon2">
                    <i class="fa-solid fa-magnifying-glass"></i>
                </div>
                <input
                    v-model.trim="search"
                    type="search"
                    class="form-control shadow-sm"
                    placeholder="Search Books"
                    aria-label="Search books"
                    aria-describedby="btnGroupAddon2"
                    @keyup.enter="submitSearch"
                >
            </div>
            <div class="fs-5" style="cursor: pointer;">
                <i class="fa-solid fa-bag-shopping"></i>
            </div>
            <div class="border p-1 px-2 border-warning-subtle rounded custom-account-btn" style="cursor: pointer;">
                <RouterLink to="/login" class="text-decoration-none text-reset">Account</RouterLink>
                
            </div>
        </div>
    </div>

    <div v-if="isOpen" class="bg-white shadow p-4 d-md-none">
        <div class="px-3 text-warning">
            <h1 class="fs-3 fw-bold">The Bookshop</h1>
        </div>
        <div class="font-monospace d-flex flex-column fw-medium gap-4 mt-3 fs-5">
            <RouterLink to="/" class="text-decoration-none text-black custom-menu-link" @click="isOpen = false">Home</RouterLink>
            <RouterLink :to="{ name: 'books' }" class="text-decoration-none text-black custom-menu-link" @click="isOpen = false">Books</RouterLink>
            <RouterLink to="/about" class="text-decoration-none text-black custom-menu-link" @click="isOpen = false">About</RouterLink>
        </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue'
    import { RouterLink, useRouter } from 'vue-router'

    const router = useRouter()
    const isOpen = ref(false)
    const search = ref('')
    const OpenMenu = () => {
        isOpen.value = !isOpen.value
    }

    function submitSearch() {
        router.push({
            name: 'books',
            query: search.value ? { search: search.value } : {}
        })
        isOpen.value = false
    }
</script>

<style scoped>
  
    .custom-menu-link {
        transition: color 0.2s ease-in-out;
        position: relative;
        padding-bottom: 2px;
    }

    .custom-menu-link:hover {
        color: #ffc107 !important; 
    }

   
    .custom-menu-link::after {
        content: '';
        position: absolute;
        width: 100%;
        transform: scaleX(0);
        height: 2px;
        bottom: 0;
        left: 0;
        background-color: #ffc107;
        transform-origin: bottom right;
        transition: transform 0.25s ease-out;
    }

    .custom-menu-link:hover::after {
        transform: scaleX(1);
        transform-origin: bottom left;
    }

   
    .custom-account-btn {
        transition: all 0.2s ease-in-out;
    }
    
    .custom-account-btn:hover {
        background-color: #ffc107;
        color: white;
        border-color: #ffc107 !important;
    }
</style>
