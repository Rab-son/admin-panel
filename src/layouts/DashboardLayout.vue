<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Mobile menu button -->
    <div class="lg:hidden fixed top-0 left-0 right-0 z-20 bg-white shadow">
      <div class="px-4 h-16 flex items-center justify-between">
        <h1 class="text-xl font-bold text-gray-900">Admin Panel</h1>
        <button
          @click="isMobileMenuOpen = !isMobileMenuOpen"
          class="p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500"
        >
          <span class="sr-only">Open sidebar</span>
          <Bars3Icon v-if="!isMobileMenuOpen" class="h-6 w-6" />
          <XMarkIcon v-else class="h-6 w-6" />
        </button>
      </div>
    </div>

    <!-- Sidebar for mobile -->
    <div v-if="isMobileMenuOpen" class="lg:hidden fixed inset-0 z-10 flex">
      <div class="fixed inset-0 bg-gray-600 bg-opacity-75" @click="isMobileMenuOpen = false"></div>
      <div class="relative flex-1 flex flex-col max-w-xs w-full bg-gray-800">
        <div class="flex-1 h-0 pt-20 pb-4 overflow-y-auto">
          <nav class="mt-5 px-2 space-y-1">
            <router-link
              v-for="item in navigationItems"
              :key="item.name"
              :to="item.to"
              class="group flex items-center px-2 py-2 text-base font-medium rounded-md text-white hover:bg-gray-700"
              active-class="bg-gray-900"
              @click="isMobileMenuOpen = false"
            >
              <component :is="item.icon" class="mr-4 h-6 w-6" />
              {{ item.name }}
            </router-link>
          </nav>
        </div>
      </div>
    </div>

    <!-- Static sidebar for desktop -->
    <div class="hidden lg:fixed lg:inset-y-0 lg:flex lg:w-64 lg:flex-col">
      <div class="flex-1 flex flex-col min-h-0 bg-gray-800">
        <div class="flex items-center h-16 flex-shrink-0 px-4 bg-gray-900">
          <h1 class="text-white text-xl font-bold">Admin Panel</h1>
        </div>
        <div class="flex-1 flex flex-col overflow-y-auto">
          <nav class="flex-1 px-2 py-4 space-y-1">
            <router-link
              v-for="item in navigationItems"
              :key="item.name"
              :to="item.to"
              class="group flex items-center px-2 py-2 text-base font-medium rounded-md text-white hover:bg-gray-700"
              active-class="bg-gray-900"
            >
              <component :is="item.icon" class="mr-4 h-6 w-6" />
              {{ item.name }}
            </router-link>
          </nav>
        </div>
      </div>
    </div>

    <!-- Main content -->
    <div class="lg:pl-64 flex flex-col flex-1">
      <!-- Top navigation -->
      <div class="sticky top-0 z-10 flex-shrink-0 flex h-16 bg-white shadow mt-16 lg:mt-0">
        <div class="flex-1 px-4 flex justify-between">
          <div class="flex-1 flex">
            <h2 class="text-2xl font-semibold text-gray-900 my-auto">
              {{ currentPage }}
            </h2>
          </div>
          <div class="ml-4 flex items-center md:ml-6">
            <button
              @click="handleLogout"
              class="flex items-center p-2 rounded-full text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              title="Logout"
            >
              <ArrowRightOnRectangleIcon class="h-6 w-6" />
              <span class="ml-2">Logout</span>
            </button>
          </div>
        </div>
      </div>

      <!-- Page content -->
      <main class="flex-1 py-6">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
          <router-view></router-view>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import {
  HomeIcon,
  UsersIcon,
  ClipboardDocumentListIcon,
  ArrowRightOnRectangleIcon,
  Bars3Icon,
  XMarkIcon,
} from '@heroicons/vue/24/outline'

const router = useRouter()
const route = useRoute()
const isMobileMenuOpen = ref(false)

const navigationItems = [
  { name: 'Dashboard', to: '/dashboard', icon: HomeIcon },
  { name: 'Users', to: '/users', icon: UsersIcon },
  { name: 'Tasks', to: '/tasks', icon: ClipboardDocumentListIcon },
]

const currentPage = computed(() => {
  return (
    route.name?.toString().charAt(0).toUpperCase() + route.name?.toString().slice(1) || 'Dashboard'
  )
})

const handleLogout = () => {
  // Clear authentication state
  localStorage.removeItem('isAuthenticated')
  localStorage.removeItem('user')

  // Redirect to login page
  router.push('/login')
}
</script>
