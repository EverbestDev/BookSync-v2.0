<template>
    <div class="max-w-5xl mx-auto px-4 sm:px-6 py-8 sm:py-12">
      <!-- Hero Section -->
      <div class="bg-gradient-to-r from-emerald-50 to-blue-50 rounded-2xl p-6 sm:p-8 mb-8 flex flex-col sm:flex-row items-center gap-6 animate-fadeIn">
        <div class="flex-shrink-0">
          <svg class="w-16 h-16 text-emerald-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
          </svg>
        </div>
        <div>
          <h2 class="text-3xl sm:text-4xl font-bold text-gray-900 mb-4">About BookSync</h2>
          <p class="text-gray-600 leading-relaxed text-base sm:text-lg">
            Discover your next great read with BookSync, your personal book discovery platform designed to inspire and organize your reading journey.
          </p>
        </div>
      </div>
  
      <!-- Mission Section -->
      <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm mb-8 animate-fadeIn">
        <h3 class="text-xl sm:text-2xl font-semibold text-gray-900 mb-4">Our Mission</h3>
        <p class="text-gray-600 leading-relaxed text-base sm:text-lg">
          At BookSync, we believe in the power of stories to connect and inspire. Our mission is to make book discovery seamless and delightful, 
          connecting you with millions of titles from the Google Books database. Whether you're a casual reader or a book enthusiast, 
          we're here to help you find your next favorite book.
        </p>
      </div>
  
      <!-- Features Section -->
      <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm animate-fadeIn">
        <h3 class="text-xl sm:text-2xl font-semibold text-gray-900 mb-6">Features</h3>
        <ul class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <li class="flex items-start text-gray-600 p-4 rounded-lg hover:bg-emerald-50 hover:shadow-md transition-all duration-300">
            <svg class="w-8 h-8 text-emerald-500 mr-4 flex-shrink-0 mt-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
            </svg>
            <span class="text-base">Search millions of books by title, author, or genre</span>
          </li>
          <li class="flex items-start text-gray-600 p-4 rounded-lg hover:bg-emerald-50 hover:shadow-md transition-all duration-300">
            <svg class="w-8 h-8 text-emerald-500 mr-4 flex-shrink-0 mt-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
            </svg>
            <span class="text-base">Save and organize your favorite books</span>
          </li>
          <li class="flex items-start text-gray-600 p-4 rounded-lg hover:bg-emerald-50 hover:shadow-md transition-all duration-300">
            <svg class="w-8 h-8 text-emerald-500 mr-4 flex-shrink-0 mt-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
            </svg>
            <span class="text-base">Explore with a stunning Pinterest-style grid</span>
          </li>
          <li class="flex items-start text-gray-600 p-4 rounded-lg hover:bg-emerald-50 hover:shadow-md transition-all duration-300">
            <svg class="w-8 h-8 text-emerald-500 mr-4 flex-shrink-0 mt-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
            </svg>
            <span class="text-base">Seamless experience across all devices</span>
          </li>
        </ul>
      </div>
  
      <!-- CTA Section -->
      <div class="text-center mt-8 animate-fadeIn">
        <button
          @click="$emit('navigate', 'books')"
          class="px-6 py-3 bg-gradient-to-r from-emerald-500 to-emerald-700 text-white font-medium rounded-xl hover:from-emerald-600 hover:to-emerald-800 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:ring-offset-2 transition-all duration-300"
        >
          Start Exploring Books
          <svg class="w-5 h-5 inline-block ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
          </svg>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineEmits } from 'vue';
  
  const emit = defineEmits(['navigate']);
  </script>
  
  <style scoped>
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  .animate-fadeIn {
    animation: fadeIn 0.6s ease-out forwards;
  }
  </style>