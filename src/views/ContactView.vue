<template>
  <div class="max-w-2xl mx-auto px-4 sm:px-6 py-8 sm:py-12">
    <h2 class="text-3xl sm:text-4xl font-bold text-gray-900 mb-6 text-center">Get in Touch</h2>

    <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm animate-slideIn">
      <!-- Contact Info -->
      <div class="space-y-4 mb-8">
        <div
          class="flex items-center text-gray-700 hover:text-emerald-600 transition-colors duration-200"
        >
          <svg
            class="w-6 h-6 text-emerald-500 mr-3 flex-shrink-0"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
            />
          </svg>
          <a href="mailto:olawooreusamahabidemi@gmail.com" class="text-base hover:underline"
            >Everbest@booksync.com</a
          >
        </div>
        <div
          class="flex items-center text-gray-700 hover:text-emerald-600 transition-colors duration-200"
        >
          <svg
            class="w-6 h-6 text-emerald-500 mr-3 flex-shrink-0"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9"
            />
          </svg>
          <a
            href="https://booksyncv2.vercell.app"
            target="_blank"
            rel="noopener noreferrer"
            class="text-base hover:underline"
            >www.booksync.com</a
          >
        </div>
        <div class="flex items-center text-gray-700">
          <svg
            class="w-6 h-6 text-emerald-500 mr-3 flex-shrink-0"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
            />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
            />
          </svg>
          <span class="text-base">Lagos, Nig.</span>
        </div>
      </div>

      <!-- Contact Form -->
      <form
        ref="contactForm"
        :action="formspreeEndpoint"
        method="POST"
        @submit.prevent="handleSubmit"
        class="space-y-6"
      >
        <div>
          <label for="name" class="block text-sm font-medium text-gray-700">Name</label>
          <input
            v-model="form.name"
            id="name"
            type="text"
            name="name"
            class="mt-1 block w-full rounded-md border border-gray-300 shadow-sm focus:ring-emerald-500 focus:border-emerald-500 sm:text-sm p-3"
            :class="{ 'border-red-500': errors.name }"
            placeholder="Your name"
          />
          <p v-if="errors.name" class="mt-1 text-sm text-red-500">{{ errors.name }}</p>
        </div>
        <div>
          <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
          <input
            v-model="form.email"
            id="email"
            type="email"
            name="_replyto"
            class="mt-1 block w-full rounded-md border border-gray-300 shadow-sm focus:ring-emerald-500 focus:border-emerald-500 sm:text-sm p-3"
            :class="{ 'border-red-500': errors.email }"
            placeholder="Your email"
          />
          <p v-if="errors.email" class="mt-1 text-sm text-red-500">{{ errors.email }}</p>
        </div>
        <div>
          <label for="message" class="block text-sm font-medium text-gray-700">Message</label>
          <textarea
            v-model="form.message"
            id="message"
            name="message"
            rows="4"
            class="mt-1 block w-full rounded-md border border-gray-300 shadow-sm focus:ring-emerald-500 focus:border-emerald-500 sm:text-sm p-3 resize-none"
            :class="{ 'border-red-500': errors.message }"
            placeholder="Your message"
          ></textarea>
          <p v-if="errors.message" class="mt-1 text-sm text-red-500">{{ errors.message }}</p>
        </div>
        <div class="flex justify-end">
          <button
            type="submit"
            class="px-6 py-2 bg-gradient-to-r from-emerald-500 to-emerald-700 text-white font-medium rounded-xl hover:from-emerald-600 hover:to-emerald-800 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:ring-offset-2 transition-all duration-200"
            :disabled="isSubmitting"
            :class="{ 'opacity-50 cursor-not-allowed': isSubmitting }"
          >
            {{ isSubmitting ? 'Sending...' : 'Send Message' }}
          </button>
        </div>
      </form>

      <!-- Success/Error Messages -->
      <div
        v-if="successMessage"
        class="mt-4 p-4 bg-green-50 border border-green-200 rounded-lg text-green-700 text-sm animate-fadeIn"
      >
        {{ successMessage }}
      </div>
      <div
        v-if="errorMessage"
        class="mt-4 p-4 bg-red-50 border border-red-200 rounded-lg text-red-700 text-sm animate-fadeIn"
      >
        {{ errorMessage }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const form = reactive({
  name: '',
  email: '',
  message: '',
})

const errors = reactive({
  name: '',
  email: '',
  message: '',
})

const isSubmitting = ref(false)
const successMessage = ref('')
const errorMessage = ref('')
const contactForm = ref(null)

const formspreeEndpoint = import.meta.env.FORMSPREE_ENDPOINT 

const handleSubmit = async () => {
  // Reset messages and errors
  errors.name = ''
  errors.email = ''
  errors.message = ''
  successMessage.value = ''
  errorMessage.value = ''

  // Basic validation
  let isValid = true
  if (!form.name.trim()) {
    errors.name = 'Name is required'
    isValid = false
  }
  if (!form.email.trim()) {
    errors.email = 'Email is required'
    isValid = false
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = 'Invalid email address'
    isValid = false
  }
  if (!form.message.trim()) {
    errors.message = 'Message is required'
    isValid = false
  }

  if (!isValid) return

  isSubmitting.value = true
  errorMessage.value = ''

  try {
    const data = new FormData(contactForm.value)
    const response = await fetch(formspreeEndpoint, {
      method: 'POST',
      body: data,
      headers: {
        Accept: 'application/json',
      },
    })

    if (response.ok) {
      successMessage.value = 'Thank you for your message! We will get back to you soon.'
      form.name = ''
      form.email = ''
      form.message = ''
    } else {
      errorMessage.value = 'There was an error sending your message. Please try again.'
    }
  } catch (error) {
    console.error('Form submission error:', error)
    errorMessage.value = 'Network error. Please check your connection and try again.'
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.animate-slideIn {
  animation: slideIn 0.6s ease-out forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fadeIn {
  animation: fadeIn 0.3s ease-out forwards;
}
</style>
