<template>
  <div class="min-h-screen flex bg-gray-50 dark:bg-gray-900">
    <!-- Left side - Login Form -->
    <div class="flex-1 flex flex-col justify-center px-4 sm:px-6 lg:px-20 xl:px-24">
      <div class="mx-auto w-full max-w-sm lg:w-96">
        <!-- Header -->
        <div>
          <div class="h-12 w-12 bg-blue-600 rounded-lg flex items-center justify-center">
            <svg class="h-6 w-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"/>
            </svg>
          </div>
          <h2 class="mt-6 text-3xl font-extrabold text-gray-900 dark:text-white">
            Sign in to your account
          </h2>
          <p class="mt-2 text-sm text-gray-600 dark:text-gray-400">
            Welcome back to Analytics Dashboard
          </p>
        </div>

        <!-- Demo Credentials Card -->
        <div class="mt-8 bg-blue-50 dark:bg-blue-900/20 border border-blue-200 dark:border-blue-800 rounded-lg p-4">
          <div class="flex">
            <div class="flex-shrink-0">
              <svg class="h-5 w-5 text-blue-400" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"/>
              </svg>
            </div>
            <div class="ml-3">
              <h3 class="text-sm font-medium text-blue-800 dark:text-blue-200">
                Demo Credentials
              </h3>
              <div class="mt-1 text-sm text-blue-700 dark:text-blue-300">
                <p><strong>Username:</strong> admin</p>
                <p><strong>Password:</strong> admin123</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Login Form -->
        <form class="mt-8 space-y-6" @submit.prevent="handleSubmit">
          <div class="space-y-5">
            <!-- Username Field -->
            <div>
              <label for="username" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                Username
              </label>
              <div class="mt-1">
                <input
                  id="username"
                  v-model="form.username"
                  type="text"
                  required
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md placeholder-gray-400 dark:placeholder-gray-500 text-gray-900 dark:text-white bg-white dark:bg-gray-800 focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  placeholder="Enter username"
                  :disabled="loading"
                />
              </div>
            </div>

            <!-- Password Field -->
            <div>
              <label for="password" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                Password
              </label>
              <div class="mt-1">
                <input
                  id="password"
                  v-model="form.password"
                  type="password"
                  required
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md placeholder-gray-400 dark:placeholder-gray-500 text-gray-900 dark:text-white bg-white dark:bg-gray-800 focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  placeholder="Enter password"
                  :disabled="loading"
                />
              </div>
            </div>

            <!-- Remember Me -->
            <div class="flex items-center justify-between">
              <div class="flex items-center">
                <input
                  id="remember-me"
                  v-model="form.rememberMe"
                  type="checkbox"
                  class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300 rounded"
                  :disabled="loading"
                />
                <label for="remember-me" class="ml-2 block text-sm text-gray-900 dark:text-gray-300">
                  Remember me
                </label>
              </div>
              <div class="text-sm">
                <a href="#" class="font-medium text-blue-600 hover:text-blue-500">
                  Forgot password?
                </a>
              </div>
            </div>
          </div>

          <!-- Error Message -->
          <div v-if="error" class="bg-red-50 dark:bg-red-900/20 border border-red-200 dark:border-red-800 rounded-md p-3">
            <p class="text-sm text-red-600 dark:text-red-400">{{ error }}</p>
          </div>

          <!-- Submit Button -->
          <div>
            <button
              type="submit"
              :disabled="loading || !form.username || !form.password"
              class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:opacity-50 disabled:cursor-not-allowed"
            >
              <span v-if="loading" class="absolute left-0 inset-y-0 flex items-center pl-3">
                <svg class="animate-spin h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
              </span>
              {{ loading ? 'Signing in...' : 'Sign in' }}
            </button>
          </div>

          <!-- Register Link -->
          <div class="text-center">
            <p class="text-sm text-gray-600 dark:text-gray-400">
              Don't have an account?
              <router-link to="/register" class="font-medium text-blue-600 hover:text-blue-500">
                Sign up
              </router-link>
            </p>
          </div>
        </form>
      </div>
    </div>

    <!-- Right side - Branding -->
    <div class="hidden lg:block relative w-0 flex-1">
      <div class="absolute inset-0 h-full w-full bg-gradient-to-br from-blue-600 via-blue-700 to-indigo-800 flex items-center justify-center">
        <div class="text-center text-white px-12">
          <h1 class="text-4xl font-bold mb-4">Analytics Dashboard</h1>
          <p class="text-xl opacity-90 mb-8">Real-time insights for your business</p>
          <div class="grid grid-cols-1 gap-4 max-w-sm mx-auto text-left">
            <div class="flex items-center">
              <svg class="h-5 w-5 mr-3" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/>
              </svg>
              <span>Real-time data visualization</span>
            </div>
            <div class="flex items-center">
              <svg class="h-5 w-5 mr-3" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/>
              </svg>
              <span>Advanced analytics tools</span>
            </div>
            <div class="flex items-center">
              <svg class="h-5 w-5 mr-3" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/>
              </svg>
              <span>Team collaboration</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth'
import { useUIStore } from '@/stores/ui'

const router = useRouter()
const authStore = useAuthStore()
const uiStore = useUIStore()

// Reactive state
const loading = ref(false)
const error = ref('')

const form = reactive({
  username: '',
  password: '',
  rememberMe: false
})

// Methods
const handleSubmit = async () => {
  try {
    loading.value = true
    error.value = ''

    await authStore.login({
      username: form.username,
      password: form.password,
      rememberMe: form.rememberMe
    })

    uiStore.showSuccess('Welcome back!')
    router.push('/')
  } catch (err: any) {
    error.value = err.message || 'Invalid credentials. Please try again.'
  } finally {
    loading.value = false
  }
}

// Auto-focus on username field
onMounted(() => {
  const usernameInput = document.getElementById('username')
  if (usernameInput) {
    usernameInput.focus()
  }
  
  // Initialize theme
  uiStore.initializePreferences()
})
</script>
