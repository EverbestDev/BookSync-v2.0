<template>
  <aside
    class="fixed md:static inset-y-0 left-0 z-40 w-20 bg-white border-r border-gray-200 transform transition-transform duration-300 ease-in-out md:transform-none overflow-x-hidden"
    :class="[isOpen ? 'translate-x-0' : '-translate-x-full md:translate-x-0']"
  >
    <div class="flex flex-col h-full">
      <!-- Logo Header -->
      <div class="flex items-center justify-center p-4 border-b border-gray-100">
        <button
          @click="$emit('navigate', 'home')"
          class="group flex items-center focus:outline-none"
        >
          <div class="relative w-10 h-10 flex items-center justify-center">
            <div
              class="absolute inset-0 bg-gradient-to-br from-emerald-500 to-emerald-600 rounded-xl transform group-hover:scale-110 transition-all duration-300 shadow-lg"
            ></div>
            <svg
              class="relative w-5 h-5 text-white z-10"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"
              />
            </svg>
          </div>
        </button>
      </div>

      <!-- Navigation -->
      <nav class="flex-1 flex flex-col items-center space-y-6 pt-6">
        <div class="relative group">
          <button
            @click="$emit('navigate', 'home')"
            class="p-3 text-gray-700 hover:text-emerald-600 hover:bg-emerald-100 rounded-xl transition-all duration-200 transform hover:scale-110"
            :class="{ 'bg-emerald-50 text-emerald-700 scale-110': activeSection === 'home' }"
            aria-label="Home"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-4 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"
              />
            </svg>
          </button>
          <span
            class="absolute left-full ml-2 top-1/2 -translate-y-1/2 bg-gray-800 text-white text-xs px-2 py-1 rounded opacity-0 group-hover:opacity-100 transition-opacity duration-200 pointer-events-none"
            >Home</span
          >
        </div>

        <div class="relative group">
          <button
            @click="$emit('navigate', 'books')"
            class="p-3 text-gray-700 hover:text-emerald-600 hover:bg-emerald-100 rounded-xl transition-all duration-200 transform hover:scale-110"
            :class="{ 'bg-emerald-50 text-emerald-700 scale-110': activeSection === 'books' }"
            aria-label="All Books"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"
              />
            </svg>
          </button>
          <span
            class="absolute left-full ml-2 top-1/2 -translate-y-1/2 bg-gray-800 text-white text-xs px-2 py-1 rounded opacity-0 group-hover:opacity-100 transition-opacity duration-200 pointer-events-none"
            >All Books</span
          >
        </div>

        <div class="relative group">
          <button
            @click="$emit('navigate', 'favorites')"
            class="p-3 text-gray-700 hover:text-emerald-600 hover:bg-emerald-100 rounded-xl transition-all duration-200 transform hover:scale-110 relative"
            :class="{ 'bg-emerald-50 text-emerald-700 scale-110': activeSection === 'favorites' }"
            aria-label="Favorites"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"
              />
            </svg>
            <span
              v-if="favoriteCount > 0"
              class="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full px-1.5 py-0.5 min-w-[20px] flex items-center justify-center"
              >{{ favoriteCount }}</span
            >
          </button>
          <span
            class="absolute left-full ml-2 top-1/2 -translate-y-1/2 bg-gray-800 text-white text-xs px-2 py-1 rounded opacity-0 group-hover:opacity-100 transition-opacity duration-200 pointer-events-none"
            >Favorites</span
          >
        </div>

        <div class="relative group">
          <button
            @click="$emit('navigate', 'about')"
            class="p-3 text-gray-700 hover:text-emerald-600 hover:bg-emerald-100 rounded-xl transition-all duration-200 transform hover:scale-110"
            :class="{ 'bg-emerald-50 text-emerald-700 scale-110': activeSection === 'about' }"
            aria-label="About"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </button>
          <span
            class="absolute left-full ml-2 top-1/2 -translate-y-1/2 bg-gray-800 text-white text-xs px-2 py-1 rounded opacity-0 group-hover:opacity-100 transition-opacity duration-200 pointer-events-none"
            >About</span
          >
        </div>

        <div class="relative group">
          <button
            @click="$emit('navigate', 'contact')"
            class="p-3 text-gray-700 hover:text-emerald-600 hover:bg-emerald-100 rounded-xl transition-all duration-200 transform hover:scale-110"
            :class="{ 'bg-emerald-50 text-emerald-700 scale-110': activeSection === 'contact' }"
            aria-label="Contact"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
              />
            </svg>
          </button>
          <span
            class="absolute left-full ml-2 top-1/2 -translate-y-1/2 bg-gray-800 text-white text-xs px-2 py-1 rounded opacity-0 group-hover:opacity-100 transition-opacity duration-200 pointer-events-none"
            >Contact</span
          >
        </div>
      </nav>

      <!-- Footer -->
      <div class="p-4 border-t border-gray-100 flex justify-center">
        <p class="text-xs text-green-500">EverbestDev © 2025</p>
      </div>
    </div>
  </aside>
</template>

<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false,
  },
  activeSection: {
    type: String,
    default: 'home',
  },
  favoriteCount: {
    type: Number,
    default: 0,
  },
})

defineEmits(['navigate', 'close'])
</script>

<style scoped>
nav {
  overflow: hidden;
}
</style>
