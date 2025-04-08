<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Sidebar -->
    <div class="fixed inset-y-0 left-0 w-64 bg-gray-800">
      <div class="flex items-center justify-center h-16 bg-gray-900">
        <h1 class="text-white text-xl font-bold">Admin Panel</h1>
      </div>
      <nav class="mt-5 px-2">
        <router-link
          to="/dashboard"
          class="group flex items-center px-2 py-2 text-base font-medium rounded-md text-white hover:bg-gray-700"
          active-class="bg-gray-900"
        >
          <HomeIcon class="mr-4 h-6 w-6" />
          Dashboard
        </router-link>
        <router-link
          to="/users"
          class="mt-1 group flex items-center px-2 py-2 text-base font-medium rounded-md text-white hover:bg-gray-700"
          active-class="bg-gray-900"
        >
          <UsersIcon class="mr-4 h-6 w-6" />
          Users
        </router-link>
        <router-link
          to="/tasks"
          class="mt-1 group flex items-center px-2 py-2 text-base font-medium rounded-md text-white hover:bg-gray-700"
          active-class="bg-gray-900"
        >
          <ClipboardDocumentListIcon class="mr-4 h-6 w-6" />
          Tasks
        </router-link>
      </nav>
    </div>

    <!-- Main content -->
    <div class="pl-64">
      <!-- Top navigation -->
      <div class="sticky top-0 z-10 flex h-16 bg-white shadow">
        <div class="flex-1 px-4 flex justify-between">
          <div class="flex-1 flex">
            <h2 class="text-2xl font-semibold text-gray-900 my-auto">
              {{ currentPage }}
            </h2>
          </div>
          <div class="ml-4 flex items-center md:ml-6">
            <button
              @click="handleLogout"
              class="p-1 rounded-full text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              <ArrowRightOnRectangleIcon class="h-6 w-6" />
            </button>
          </div>
        </div>
      </div>

      <!-- Page content -->
      <main class="py-6">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
          <router-view></router-view>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import {
  HomeIcon,
  UsersIcon,
  ClipboardDocumentListIcon,
  ArrowRightOnRectangleIcon,
} from '@heroicons/vue/24/outline'

const router = useRouter()
const route = useRoute()

const currentPage = computed(() => {
  return (
    route.name?.toString().charAt(0).toUpperCase() + route.name?.toString().slice(1) || 'Dashboard'
  )
})

const handleLogout = () => {
  // TODO: Implement actual logout logic
  router.push('/login')
}
</script>
