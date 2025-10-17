<template>
    <header class="sticky top-0 z-20 bg-white/95 backdrop-blur-md border-b border-gray-200 shadow-sm">
      <div class="flex items-center gap-3 px-4 py-3 md:px-6 md:py-4">
        
        <!-- Mobile Menu Toggle Button -->
        <button
          v-if="false"
          @click="$emit('toggle-sidebar')"
          class="md:hidden p-2.5 text-gray-600 hover:text-emerald-600 hover:bg-gray-100 rounded-xl transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-emerald-500"
          aria-label="Toggle sidebar menu"
        >
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
          </svg>
        </button>
  
        <!-- Search Input -->
        <div class="relative flex-1 group">
          <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none">
            <svg class="w-5 h-5 text-gray-400 group-focus-within:text-emerald-500 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
            </svg>
          </div>
          
          <input
            type="text"
            v-model="localSearchTerm"
            @keyup.enter="triggerSearch"
            :disabled="isLoading"
            class="w-full pl-12 pr-4 py-3 bg-gray-50 border border-gray-200 rounded-xl text-sm text-gray-900 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:border-transparent focus:bg-white transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
            placeholder="Search books, authors, ISBN..."
            aria-label="Search for books"
          />
  
          <!-- Loading Spinner -->
          <div v-if="isLoading" class="absolute inset-y-0 right-0 pr-4 flex items-center">
            <svg class="animate-spin h-5 w-5 text-emerald-500" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
          </div>
  
          <!-- Enter Key Hint -->
          <div 
            v-if="!isLoading && localSearchTerm.length > 0" 
            class="absolute inset-y-0 right-0 pr-4 flex items-center pointer-events-none md:hidden"
          >
            <kbd class="px-2 py-1 text-xs font-semibold text-gray-500 bg-gray-100 border border-gray-200 rounded">
              Enter
            </kbd>
          </div>
        </div>
  
        <!-- Search Button-->
        <button
          @click="triggerSearch"
          :disabled="isLoading"
          class="hidden md:flex items-center px-6 py-3 bg-gradient-to-r from-emerald-500 to-emerald-600 text-white font-medium rounded-xl hover:from-emerald-600 hover:to-emerald-700 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:ring-offset-2 transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed shadow-sm hover:shadow-md whitespace-nowrap"
        >
          Search
        </button>
  
        <!-- Filter Icon Button -->
        <div class="relative">
          <button
            @click="toggleFilterMenu"
            :disabled="isLoading"
            class="flex items-center gap-1 px-3 py-3 text-gray-600 hover:text-emerald-600 hover:bg-gray-100 rounded-xl transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-emerald-500"
            aria-label="Filter options"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 4a1 1 0 011-1h16a1 1 0 011 1v2.586a1 1 0 01-.293.707l-6.414 6.414a1 1 0 00-.293.707V17l-4 4v-6.586a1 1 0 00-.293-.707L3.293 7.293A1 1 0 013 6.586V4z"/>
            </svg>
            <span v-if="filterTags" class="text-xs font-medium text-emerald-600">{{ filterTags }}</span>
          </button>
  
          <!-- Popover Menu -->
          <Transition name="fade">
            <div v-if="showFilterMenu" class="absolute right-0 mt-2 w-64 bg-white rounded-xl shadow-lg border border-gray-200 p-4 z-50 md:w-80">
              <div class="space-y-4">
                <!-- Category Select -->
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Category</label>
                  <select
                    v-model="selectedCategory"
                    class="w-full px-4 py-2 bg-gray-50 border border-gray-200 rounded-xl text-sm text-gray-900 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:border-transparent transition-all duration-200"
                    @change="triggerSearchAndClose"
                  >
                    <option value="">All Categories</option>
                    <option value="romance">Romance</option>
                    <option value="tragedy">Tragedy</option>
                    <option value="science">Science</option>
                    <option value="psychology">Psychology</option>
                    <option value="programming">Programming</option>
                    <option value="fiction">Fiction</option>
                    <option value="nonfiction">Non-Fiction</option>
                    <option value="mystery">Mystery</option>
                    <option value="fantasy">Fantasy</option>
                    <option value="biography">Biography</option>
                  </select>
                </div>
  
                <!-- Sort Select -->
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Sort By</label>
                  <select
                    v-model="selectedSort"
                    class="w-full px-4 py-2 bg-gray-50 border border-gray-200 rounded-xl text-sm text-gray-900 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:border-transparent transition-all duration-200"
                    @change="triggerSearchAndClose"
                  >
                    <option value="">Relevance</option>
                    <option value="newest">Latest Published</option>
                    <option value="relevance">Most Relevant</option>
                  </select>
                </div>
  
                <button
                  @click="showFilterMenu = false"
                  class="w-full px-4 py-2 bg-gray-100 text-gray-700 font-medium rounded-xl hover:bg-gray-200 transition-all duration-200"
                >
                  Close
                </button>
              </div>
            </div>
          </Transition>
        </div>
  
      </div>
    </header>
  </template>
  
  <script setup>
  import { ref, watch, computed } from 'vue';
  
  const props = defineProps({
    modelValue: {
      type: String,
      default: ''
    },
    isLoading: {
      type: Boolean,
      default: false
    }
  });
  
  const emit = defineEmits(['update:modelValue', 'search', 'toggle-sidebar']);
  
  const localSearchTerm = ref(props.modelValue);
  const selectedCategory = ref('');
  const selectedSort = ref('');
  const showFilterMenu = ref(false);
  
  watch(() => props.modelValue, (newValue) => {
    localSearchTerm.value = newValue;
  });
  
  watch(localSearchTerm, (newValue) => {
    emit('update:modelValue', newValue);
  });
  
  // Trigger search
  const triggerSearch = () => {
    const trimmedTerm = localSearchTerm.value.trim();
    emit('search', {
      query: trimmedTerm || 'all',
      category: selectedCategory.value,
      sort: selectedSort.value
    });
  };
  
  // search and close menu
  const triggerSearchAndClose = () => {
    triggerSearch();
    showFilterMenu.value = false;
  };
  
  // Toggle filter menu
  const toggleFilterMenu = () => {
    showFilterMenu.value = !showFilterMenu.value;
  };
  
  const filterTags = computed(() => {
    const tags = [];
    if (selectedCategory.value) tags.push(selectedCategory.value.charAt(0).toUpperCase() + selectedCategory.value.slice(1));
    if (selectedSort.value) tags.push(selectedSort.value === 'newest' ? 'Newest' : 'Relevant');
    return tags.join(' • ');
  });
  </script>
  
  <style scoped>

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
  
  .animate-spin {
    animation: spin 1s linear infinite;
  }
  
  select {
    background-image: none;
    -webkit-appearance: none;
    -moz-appearance: none;
  }
  

  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.3s ease;
  }
  .fade-enter-from, .fade-leave-to {
    opacity: 0;
  }
  </style>