<template>
    <div class="max-w-[2000px] mx-auto px-4 py-6">
      
      <!-- Page Header -->
      <div class="mb-6">
        <h2 class="text-2xl md:text-3xl font-bold text-gray-900 flex items-center mb-2">
          <svg class="w-7 h-7 mr-3 text-green-500 fill-current" viewBox="0 0 24 24">
            <path d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
          </svg>
          My Favorite Books
        </h2>
        <p class="text-gray-600 ml-10">
          {{ favorites.length }} book{{ favorites.length !== 1 ? 's' : '' }} saved
        </p>
      </div>
      
      <!-- Empty State -->
      <div v-if="favorites.length === 0" class="flex flex-col items-center justify-center py-20 px-4">
        <div class="w-20 h-20 bg-green-100 rounded-full flex items-center justify-center mb-4">
          <svg class="w-10 h-10 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
          </svg>
        </div>
        <h3 class="text-xl font-semibold text-gray-900 mb-2">No favorites yet</h3>
        <p class="text-gray-600 text-center max-w-md mb-6">
          Start exploring and click the heart icon on books you love to save them here
        </p>
        <button
          @click="$emit('navigate', 'home')"
          class="px-6 py-3 bg-gradient-to-r from-emerald-500 to-emerald-600 text-white font-medium rounded-xl hover:from-emerald-600 hover:to-emerald-700 transition-all duration-200 shadow-sm hover:shadow-md">
          Explore Books
        </button>
      </div>
  
      <!-- Favorites Grid -->
      <MasonryGrid
      :books="favoriteBooksWithStatus"
      :is-loading="false"
      @book-click="$emit('book-click', $event)"
      @toggle-favorite="$emit('toggle-favorite', $event)"
    />
  
    </div>
  </template>
  
  <script setup>
  import MasonryGrid from '../components/MasonryGrid.vue';
  import { computed } from 'vue';

const props = defineProps({
  favorites: {
    type: Array,
    default: () => []
  }
});

const favoriteBooksWithStatus = computed(() => {
  return props.favorites.map(book => ({
    ...book,
    isFavorite: true 
  }));
});
  </script>