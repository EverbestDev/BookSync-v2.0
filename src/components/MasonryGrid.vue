<template>
    <div class="masonry-container">
      
      <!-- Loading State -->
      <div v-if="isLoading" class="flex justify-center items-center py-20">
        <div class="text-center">
          <svg class="animate-spin h-12 w-12 text-emerald-500 mx-auto mb-4" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
          </svg>
          <p class="text-gray-600 font-medium">Discovering books...</p>
        </div>
      </div>
  
      <!-- Empty State -->
      <div v-else-if="books.length === 0" class="flex flex-col items-center justify-center py-20 px-4">
        <div class="w-20 h-20 bg-emerald-100 rounded-full flex items-center justify-center mb-4">
          <svg class="w-10 h-10 text-emerald-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
          </svg>
        </div>
        <h3 class="text-xl font-semibold text-gray-900 mb-2">No books found</h3>
        <p class="text-gray-600 text-center max-w-md">
          Try searching for a different title, author, or subject
        </p>
      </div>
  
      <!-- Uniform Grid -->
      <div v-else class="uniform-grid">
        <div
          v-for="book in books"
          :key="book.key"
          class="book-card-wrapper group transition-all duration-500 hover:rotate-1"
        >
          
          <!-- Book Card -->
          <div class="book-card bg-white rounded-2xl overflow-hidden shadow-xl hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-2 h-full flex flex-col border border-transparent hover:border-emerald-300 hover:bg-gradient-to-br hover:from-emerald-50 hover:to-blue-50">
            
            <!-- Book Cover Image -->
            <div class="relative h-80 bg-gradient-to-br from-emerald-100 via-emerald-50 to-blue-50 overflow-hidden flex-shrink-0">
              
              <!-- Cover Image -->
              <img 
                v-if="getCoverUrl(book)"
                :src="getCoverUrl(book)"
                :alt="book.title"
                class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
                loading="lazy"
                @error="(e) => e.target.style.display = 'none'"
              />
              
              <div v-else class="w-full h-full flex flex-col items-center justify-center p-6">
                <div class="w-32 h-32 bg-gradient-to-br from-emerald-500 to-emerald-700 rounded-2xl flex items-center justify-center mb-3 shadow-lg transform group-hover:scale-105 transition-transform duration-300">
                  <span class="text-5xl font-bold text-white">
                    {{ getFirstLetter(book.title) }}
                  </span>
                </div>
                <svg class="w-16 h-16 text-emerald-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
                </svg>
              </div>

              <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
  
              <!-- Favorite Button -->
              <button
                @click.stop="toggleFavorite(book)"
                class="absolute top-3 right-3 p-2.5 bg-white/95 backdrop-blur-sm rounded-full shadow-lg opacity-100 group-hover:opacity-100 transition-all duration-300 hover:scale-110 focus:outline-none focus:ring-2 focus:ring-emerald-500 z-10"
                :aria-label="book.isFavorite ? 'Remove from favorites' : 'Add to favorites'"
              >
                <svg 
                  class="w-5 h-5 transition-all duration-200"
                  :class="[book.isFavorite ? 'fill-green-500 text-green-500' : 'fill-none text-gray-600']"
                  stroke="currentColor"
                  stroke-width="2"
                  viewBox="0 0 24 24"
                >
                  <path stroke-linecap="round" stroke-linejoin="round" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
                </svg>
              </button>
            </div>
  
            <!-- Book Content -->
            <div class="p-6 flex flex-col flex-1">
              <h3 class="text-xl font-bold text-gray-900 mb-2 line-clamp-2">{{ book.title }}</h3>
              <p class="text-sm text-gray-600 mb-1 line-clamp-1">By: {{ formatAuthors(book) }}</p>
              <p class="text-xs text-gray-500 mb-4">Year: {{ formatYear(book) }}</p>
              <p class="text-sm text-gray-700 flex-1 line-clamp-3 mb-4">
                {{ truncate(book.description, 120) }}
              </p>
  
              <!-- View Details -->
              <button
                @click="$emit('book-click', book)"
                class="px-4 py-2 bg-gradient-to-r from-emerald-500 to-emerald-700 text-white font-medium rounded-xl hover:from-emerald-600 hover:to-emerald-700 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:ring-offset-2 transition-all duration-200 shadow-sm hover:shadow-md transform hover:scale-105 active:scale-95"
              >
                View Details
                <svg class="w-4 h-4 inline-block ml-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
                </svg>
              </button>
            </div>
  
          </div>
        </div>
      </div>
  
    </div>
  </template>
  
  <script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  books: {
    type: Array,
    default: () => []
  },
  isLoading: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['book-click', 'toggle-favorite']);

const getCoverUrl = (book) => book.cover_url || null;

const formatYear = (book) => book.published_year || 'N/A';

const formatAuthors = (book) => book.author || 'Unknown Author';

const getFirstLetter = (title) => title ? title.charAt(0).toUpperCase() : '?';

const truncate = (text, length) => {
  if (!text) return 'No description available';
  const cleanText = text.replace(/<[^>]*>?/gm, '');
  return cleanText.length > length ? cleanText.substring(0, length) + '...' : cleanText;
};

const toggleFavorite = (book) => {
  emit('toggle-favorite', book);
};
</script>
  
  <style scoped>
  .masonry-container {
  width: 100%;
}

.uniform-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
  width: 100%;
}
  

  @media (max-width: 639px) {
    .uniform-grid {
      grid-template-columns: 1fr;
      gap: 1rem;
    }
  }
  
 
  @media (min-width: 640px) and (max-width: 1023px) {
    .uniform-grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 1.25rem;
    }
  }
  
 
  @media (min-width: 1024px) and (max-width: 1279px) {
    .uniform-grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  

  @media (min-width: 1280px) and (max-width: 1535px) {
    .uniform-grid {
      grid-template-columns: repeat(4, 1fr);
    }
  }
  

  @media (min-width: 1536px) {
    .uniform-grid {
      grid-template-columns: repeat(5, 1fr);
    }
  }
  .book-card {
  height: 100%;
  min-height: 600px;  
}
  
  /* Text truncation */
  .line-clamp-1 {
    display: -webkit-box;
    line-clamp: 1;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  
  .line-clamp-2 {
    display: -webkit-box;
    line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  
  .line-clamp-3 {
    display: -webkit-box;
    line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  @keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
.animate-spin {
  animation: spin 1s linear infinite;
}
  </style>