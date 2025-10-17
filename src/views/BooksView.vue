<template>
  <div class="max-w-[2000px] mx-auto px-4 py-6">
    
    <div class="mb-4">
      <h2 class="text-3xl md:text-4xl font-bold text-gray-900">All Books</h2>
      <p class="text-base text-gray-600 mt-1">{{ books.length }} books available</p>
    </div>
    
    <MasonryGrid 
      :books="books"
      :is-loading="isLoading"
      @book-click="$emit('book-click', $event)"
      @toggle-favorite="$emit('toggle-favorite', $event)"
    />

    <!-- Infinite Scroll Trigger -->
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
  isLoading: Boolean
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
      setTimeout(() => { isLoadingMore.value = false; }, 1000);
    }
  }, { threshold: 0.1 });

  if (loadMore.value) observer.observe(loadMore.value);
});

onUnmounted(() => {
  if (observer && loadMore.value) observer.unobserve(loadMore.value);
});
</script>