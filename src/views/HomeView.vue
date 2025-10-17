<template>
    <div class="max-w-[2000px] mx-auto px-4 py-6">
      
      <!-- Hero Section -->
      <section v-if="!searchTerm" class="mb-8 animate-fadeIn">
        <div class="text-center py-8 md:py-12">
          <h1 class="text-5xl md:text-6xl lg:text-7xl font-extrabold text-gray-900 mb-4 drop-shadow-lg">
            Discover Your Next
            <span class="bg-gradient-to-r from-emerald-600 to-emerald-800 bg-clip-text text-transparent animate-pulse">
              Great Read
            </span>
          </h1>
          <p class="text-xl md:text-2xl text-gray-600 max-w-3xl mx-auto mb-2 font-medium italic">
            Search millions of books, save your favorites, and explore curated collections
          </p>
          <p class="text-base text-emerald-500 font-semibold">
            {{ books.length }} books loaded
          </p>
        </div>
      </section>
  
      <!-- Search Results Header -->
      <div v-if="searchTerm" class="mb-4">
        <h2 class="text-2xl font-semibold text-gray-900">
          Search results for "<span class="text-emerald-600">{{ searchTerm }}</span>"
        </h2>
        <p class="text-sm text-gray-600 mt-1">{{ books.length }} books found</p>
      </div>
      
      <!-- Books Grid -->
      <MasonryGrid 
        :books="books"
        :is-loading="isLoading"
        @book-click="$emit('book-click', $event)"
        @toggle-favorite="$emit('toggle-favorite', $event)"
      />
  
      <!-- Infinite Scroll -->
      <div ref="loadMore" class="h-10 flex justify-center items-center">
        <svg v-if="isLoadingMore" class="animate-spin h-8 w-8 text-emerald-500" fill="none" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        <p v-if="isLoadingMore" class="text-gray-600 ml-2">Loading more books...</p>
      </div>
  
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  import MasonryGrid from '../components/MasonryGrid.vue';
  
  const props = defineProps({
    books: Array,
    isLoading: Boolean,
    searchTerm: String
  });
  
  const emit = defineEmits(['book-click', 'toggle-favorite', 'load-more']);
  
  const loadMore = ref(null);
  const isLoadingMore = ref(false);
  
  let observer = null;
  
  onMounted(() => {
    observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting && !props.isLoading) {
        isLoadingMore.value = true;
        emit('load-more');
        setTimeout(() => { isLoadingMore.value = false; }, 1000);  // Simulate delay; remove if not needed
      }
    }, { threshold: 0.1 });
  
    if (loadMore.value) observer.observe(loadMore.value);
  });
  
  onUnmounted(() => {
    if (observer && loadMore.value) observer.unobserve(loadMore.value);
  });
  </script>
  
  <style scoped>
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  .animate-fadeIn {
    animation: fadeIn 1s ease-out;
  }
  
  .animate-pulse {
    animation: pulse 2s infinite;
  }
  
  @keyframes pulse {
    0% { opacity: 1; }
    50% { opacity: 0.8; }
    100% { opacity: 1; }
  }
  </style>