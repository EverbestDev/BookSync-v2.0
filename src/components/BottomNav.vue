<template>
    <nav class="fixed bottom-0 left-0 right-0 z-40 bg-white border-t border-gray-200 shadow-md flex justify-around items-center py-2 px-4 md:hidden">
      <button
        @click="$emit('navigate', 'home')"
        class="flex flex-col items-center text-gray-700 hover:text-emerald-700 transition-colors"
        :class="{ 'text-emerald-700': activeSection === 'home' }"
        aria-label="Home"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-4 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/>
        </svg>
        <span class="text-xs mt-1">Home</span>
      </button>
  
      <button
        @click="$emit('navigate', 'books')"
        class="flex flex-col items-center text-gray-700 hover:text-emerald-700 transition-colors"
        :class="{ 'text-emerald-700': activeSection === 'books' }"
        aria-label="All Books"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
        </svg>
        <span class="text-xs mt-1">Books</span>
      </button>
  
      <button
        @click="$emit('navigate', 'favorites')"
        class="relative flex flex-col items-center text-gray-700 hover:text-emerald-700 transition-colors"
        :class="{ 'text-emerald-700': activeSection === 'favorites' }"
        aria-label="Favorites"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
        </svg>
        <span v-if="favoriteCount > 0" class="absolute top-0 right-0 bg-green-500 text-white text-xs rounded-full px-1 min-w-[16px]">{{ favoriteCount }}</span>
        <span class="text-xs mt-1">Favorites</span>
      </button>
  
      <button
        @click="$emit('navigate', 'about')"
        class="flex flex-col items-center text-gray-700 hover:text-emerald-700 transition-colors"
        :class="{ 'text-emerald-700': activeSection === 'about' }"
        aria-label="About"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
        </svg>
        <span class="text-xs mt-1">About</span>
      </button>
  
      <button
        @click="$emit('navigate', 'contact')"
        class="flex flex-col items-center text-gray-700 hover:text-emerald-700 transition-colors"
        :class="{ 'text-emerald-700': activeSection === 'contact' }"
        aria-label="Contact"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
        </svg>
        <span class="text-xs mt-1">Contact</span>
      </button>
    </nav>
  </template>
  
  <script setup>
  defineProps({
    activeSection: {
      type: String,
      default: 'home'
    },
    favoriteCount: {
      type: Number,
      default: 0
    }
  });
  
  defineEmits(['navigate']);
  </script>
  
  <style scoped>
  </style>