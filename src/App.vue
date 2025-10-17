<template>
  <div class="flex h-screen bg-gray-50 overflow-hidden">
    <!-- Desktop Sidebar -->
    <Sidebar
      class="hidden md:block"
      :is-open="true"
      :active-section="currentSection"
      :favorite-count="favoriteBooks.length"
      @navigate="handleNavigation"
    />

    <!-- Main Content Area -->
    <div class="flex-1 flex flex-col overflow-hidden pb-16 md:pb-0">
      <!-- Search Bar Header -->
      <SearchBar
        v-model="searchTerm"
        :is-loading="isSearching"
        @search="handleSearch"
        @toggle-sidebar="sidebarOpen = !sidebarOpen"
      />

      <!-- Dynamic Content Views -->
      <main class="flex-1 overflow-y-auto bg-gray-50">
        <component
          :is="currentView"
          :books="displayedBooks"
          :favorites="favoriteBooks"
          :is-loading="isSearching"
          :search-term="searchTerm"
          @book-click="handleBookClick"
          @toggle-favorite="handleToggleFavorite"
          @navigate="handleNavigation"
          @load-more="loadMore"
        />
      </main>
    </div>

    <!-- Mobile Bottom Navigation -->
    <BottomNav
      class="md:hidden"
      :active-section="currentSection"
      :favorite-count="favoriteBooks.length"
      @navigate="handleNavigation"
    />

    <!-- Book Viewer Modal -->
    <Transition name="slide-up">
      <div
        v-if="selectedBook"
        class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 transition-opacity duration-300"
      >
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md p-6 relative animate-slideUp">
          <button
            @click="closeModal"
            class="absolute top-4 right-4 p-2 text-gray-600 hover:text-emerald-600"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
          <div class="text-center">
            <h3 class="text-xl font-semibold text-gray-900 mb-4">{{ selectedBook.title }}</h3>
            <p class="text-gray-600 mb-6">
              In-app book viewing is still in development. Would you like to read this book on
              Google Books?
            </p>
            <a
              :href="`https://books.google.com/books?id=${selectedBook.key}`"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-block px-6 py-2 bg-gradient-to-r from-emerald-500 to-emerald-600 text-white font-medium rounded-xl hover:from-emerald-600 hover:to-emerald-700 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:ring-offset-2 transition-all duration-200"
            >
              Read on Google Books
            </a>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Random Book Toast -->
    <Transition name="fade">
      <div
        v-if="showToast"
        class="fixed bottom-4 md:bottom-20 left-0 md:left-1/2 md:-translate-x-1/2 z-40 w-full md:max-w-md bg-gradient-to-r from-emerald-50 to-blue-50 rounded-xl shadow-lg p-4 mx-2 md:mx-0 flex flex-col md:flex-row items-start md:items-center gap-3 animate-fadeIn"
      >
        <img
          :src="randomBook.cover_url"
          alt="Random Book"
          class="w-full object-center h-24 md:w-12 md:h-16 object-cover rounded-lg shadow-sm"
        />
        <div class="flex-1">
          <p class="text-sm font-semibold text-gray-900 line-clamp-1">
            Recommended: {{ randomBook.title }}
          </p>
          <p class="text-xs text-gray-600 line-clamp-1">By: {{ randomBook.author }}</p>
        </div>
        <div class="flex items-center gap-2 w-full md:w-auto">
          <button
            @click="viewRandomBook"
            class="flex-1 md:flex-none px-4 py-1.5 bg-emerald-500 text-white rounded-lg text-sm font-medium hover:bg-emerald-600 transition-all duration-200"
          >
            View
          </button>
          <button
            @click="closeToast"
            class="p-2 text-gray-500 hover:text-gray-700 rounded-full hover:bg-gray-200 transition-all duration-200"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, computed, defineAsyncComponent, onMounted } from 'vue'
import Sidebar from './components/Sidebar.vue'
import BottomNav from './components/BottomNav.vue'
import SearchBar from './components/SearchBar.vue'

// Async components
const HomeView = defineAsyncComponent(() => import('./views/HomeView.vue'))
const BooksView = defineAsyncComponent(() => import('./views/BooksView.vue'))
const FavoritesView = defineAsyncComponent(() => import('./views/FavoritesView.vue'))
const AboutView = defineAsyncComponent(() => import('./views/AboutView.vue'))
const ContactView = defineAsyncComponent(() => import('./views/ContactView.vue'))

// STATE
const books = ref([])
const favoriteBooks = ref([])
const currentSection = ref('home')
const searchTerm = ref('')
const isSearching = ref(false)
const sidebarOpen = ref(false)
const selectedBook = ref(null)
const showToast = ref(false)
const randomBook = ref(null)
const currentStartIndex = ref(0)
const hasMore = ref(true)
const latestQuery = ref({ query: 'bestsellers', category: '', sort: '' })

// CONSTANTS
const GOOGLE_API_URL = process.env.VITE_GOOGLE_API_URL
const API_KEY = process.env.VITE_API_KEY
const MAX_RETRIES = process.env.VITE_MAX_RETRIES

// COMPUTED
const currentView = computed(() => {
  switch (currentSection.value) {
    case 'home':
      return HomeView
    case 'books':
      return BooksView
    case 'favorites':
      return FavoritesView
    case 'about':
      return AboutView
    case 'contact':
      return ContactView
    default:
      return HomeView
  }
})

const displayedBooks = computed(() => {
  return books.value.map((book) => ({
    ...book,
    isFavorite: favoriteBooks.value.some((fav) => fav.key === book.key),
  }))
})

// LIFECYCLE
onMounted(() => {
  loadFavorites()
  handleSearch(latestQuery.value, true)
})

// UTILITIES
const fetchWithRetry = async (url, retries = MAX_RETRIES) => {
  for (let i = 0; i < retries; i++) {
    try {
      const response = await fetch(url)
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }
      return response.json()
    } catch (error) {
      if (i < retries - 1) {
        await new Promise((resolve) => setTimeout(resolve, Math.pow(2, i) * 1000))
      } else {
        throw error
      }
    }
  }
}

const loadFavorites = () => {
  try {
    const savedFavorites = localStorage.getItem('bookSyncFavorites')
    if (savedFavorites) {
      favoriteBooks.value = JSON.parse(savedFavorites)
    }
  } catch (e) {
    console.error('Could not load favorites from storage:', e)
  }
}

const saveFavorites = () => {
  try {
    localStorage.setItem('bookSyncFavorites', JSON.stringify(favoriteBooks.value))
  } catch (e) {
    console.error('Could not save favorites to storage:', e)
  }
}

// API CALL
const handleSearch = async (params, isInitialLoad = false, append = false) => {
  isSearching.value = true

  if (!isInitialLoad) {
    searchTerm.value = params.query
    latestQuery.value = params
  }

  const startIndex = append ? books.value.length : 0

  try {
    let url = `${GOOGLE_API_URL}?q=${encodeURIComponent(params.query)}&startIndex=${startIndex}`
    if (params.category) url += `+subject:${encodeURIComponent(params.category)}`
    if (params.sort) url += `&orderBy=${encodeURIComponent(params.sort)}`
    url += `&maxResults=40`
    if (API_KEY) url += `&key=${API_KEY}`

    const data = await fetchWithRetry(url)

    const newBooks = (data.items || [])
      .map((item) => {
        const info = item.volumeInfo
        return {
          key: item.id,
          title: info.title || 'Untitled',
          author: info.authors ? info.authors.join(', ') : 'Unknown Author',
          cover_url:
            info.imageLinks?.thumbnail ||
            'https://placehold.co/128x193/f3f4f6/374151?text=No+Cover',
          published_year: info.publishedDate ? info.publishedDate.substring(0, 4) : 'N/A',
          description: info.description || '',
        }
      })
      .filter((book) => book.title)

    if (append) {
      books.value.push(...newBooks)
    } else {
      books.value = newBooks
    }

    hasMore.value = data.totalItems > startIndex + (data.items?.length || 0)

    if (!isInitialLoad && currentSection.value !== 'home' && currentSection.value !== 'books') {
      currentSection.value = 'home'
    }

    if (isInitialLoad && books.value.length > 0) {
      setTimeout(showRandomToast, 5000)
    }
  } catch (error) {
    console.error('Search error:', error)
    if (!append) books.value = []
  } finally {
    isSearching.value = false
  }
}

// Load More for Infinite Scroll
const loadMore = () => {
  if (hasMore.value && !isSearching.value) {
    handleSearch(latestQuery.value, false, true)
  }
}

// EVENT HANDLERS
const handleNavigation = (section) => {
  currentSection.value = section
  sidebarOpen.value = false
  if (section !== 'home' && section !== 'books') {
    searchTerm.value = ''
  }
}

const handleBookClick = (book) => {
  selectedBook.value = book
}

const closeModal = () => {
  selectedBook.value = null
}

// Toast Logic
const showRandomToast = () => {
  if (books.value.length > 0) {
    const index = Math.floor(Math.random() * books.value.length)
    randomBook.value = books.value[index]
    showToast.value = true
    setTimeout(closeToast, 10000)
  }
}

const closeToast = () => {
  showToast.value = false
}

const viewRandomBook = () => {
  handleBookClick(randomBook.value)
  closeToast()
}

const handleToggleFavorite = (book) => {
  const fullBook = books.value.find((b) => b.key === book.key) || book
  const index = favoriteBooks.value.findIndex((fav) => fav.key === fullBook.key)

  if (index > -1) {
    favoriteBooks.value.splice(index, 1)
  } else {
    favoriteBooks.value.push(fullBook)
  }

  saveFavorites()
}
</script>

<style>
/* Modal and Toast Animations */
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.3s ease;
}
.slide-up-enter-from,
.slide-up-leave-to {
  opacity: 0;
  transform: translateY(100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.animate-slideUp {
  animation: slideUp 0.5s ease-out;
}

@keyframes slideUp {
  from {
    transform: translateY(50px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.animate-fadeIn {
  animation: fadeIn 1s ease-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
