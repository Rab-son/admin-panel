<template>
  <div>
    <div class="sm:flex sm:items-center">
      <div class="sm:flex-auto">
        <h1 class="text-xl font-semibold text-gray-900">Tasks</h1>
        <p class="mt-2 text-sm text-gray-700">
          A list of all tasks in the system including their title, status, and due date.
        </p>
      </div>
      <div class="mt-4 sm:mt-0 sm:ml-16 sm:flex-none">
        <button
          @click="openCreateModal"
          class="inline-flex items-center justify-center rounded-md border border-transparent bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto"
        >
          Add task
        </button>
      </div>
    </div>

    <!-- Tasks table -->
    <div class="mt-8 flex flex-col">
      <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="inline-block min-w-full py-2 align-middle md:px-6 lg:px-8">
          <div class="overflow-hidden shadow ring-1 ring-black ring-opacity-5 md:rounded-lg">
            <table class="min-w-full divide-y divide-gray-300">
              <thead class="bg-gray-50">
                <tr>
                  <th
                    scope="col"
                    class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6"
                  >
                    Title
                  </th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                    Description
                  </th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                    Status
                  </th>
                  <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                    Due Date
                  </th>
                  <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-6">
                    <span class="sr-only">Actions</span>
                  </th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-200 bg-white">
                <tr v-for="task in tasks" :key="task.id">
                  <td
                    class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6"
                  >
                    {{ task.title }}
                  </td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                    {{ task.description }}
                  </td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                    <span
                      :class="{
                        'bg-green-100 text-green-800': task.status === 'Completed',
                        'bg-yellow-100 text-yellow-800': task.status === 'In Progress',
                        'bg-red-100 text-red-800': task.status === 'Pending',
                      }"
                      class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full"
                    >
                      {{ task.status }}
                    </span>
                  </td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                    {{ task.dueDate }}
                  </td>
                  <td
                    class="relative whitespace-nowrap py-4 pl-3 pr-4 text-right text-sm font-medium sm:pr-6"
                  >
                    <button
                      @click="openEditModal(task)"
                      class="text-indigo-600 hover:text-indigo-900 mr-4"
                    >
                      Edit
                    </button>
                    <button @click="deleteTask(task.id)" class="text-red-600 hover:text-red-900">
                      Delete
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>

    <!-- Create/Edit Modal -->
    <div
      v-if="showModal"
      class="fixed inset-0 bg-gray-500 bg-opacity-75 flex items-center justify-center"
    >
      <div
        class="bg-white rounded-lg px-4 pt-5 pb-4 overflow-hidden shadow-xl transform transition-all sm:max-w-lg sm:w-full sm:p-6"
      >
        <div class="sm:flex sm:items-start">
          <div class="mt-3 text-center sm:mt-0 sm:text-left w-full">
            <h3 class="text-lg leading-6 font-medium text-gray-900">
              {{ editingTask ? 'Edit Task' : 'Create Task' }}
            </h3>
            <div class="mt-4">
              <form @submit.prevent="handleSubmit">
                <div class="space-y-4">
                  <div>
                    <label for="title" class="block text-sm font-medium text-gray-700">Title</label>
                    <input
                      type="text"
                      id="title"
                      v-model="formData.title"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
                    />
                  </div>
                  <div>
                    <label for="description" class="block text-sm font-medium text-gray-700"
                      >Description</label
                    >
                    <textarea
                      id="description"
                      v-model="formData.description"
                      rows="3"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
                    ></textarea>
                  </div>
                  <div>
                    <label for="status" class="block text-sm font-medium text-gray-700"
                      >Status</label
                    >
                    <select
                      id="status"
                      v-model="formData.status"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
                    >
                      <option value="Pending">Pending</option>
                      <option value="In Progress">In Progress</option>
                      <option value="Completed">Completed</option>
                    </select>
                  </div>
                  <div>
                    <label for="dueDate" class="block text-sm font-medium text-gray-700"
                      >Due Date</label
                    >
                    <input
                      type="date"
                      id="dueDate"
                      v-model="formData.dueDate"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
                    />
                  </div>
                </div>
                <div class="mt-5 sm:mt-4 sm:flex sm:flex-row-reverse">
                  <button
                    type="submit"
                    class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-indigo-600 text-base font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:ml-3 sm:w-auto sm:text-sm"
                  >
                    {{ editingTask ? 'Update' : 'Create' }}
                  </button>
                  <button
                    @click="closeModal"
                    class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:w-auto sm:text-sm"
                  >
                    Cancel
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Task {
  id: number
  title: string
  description: string
  status: string
  dueDate: string
}

const tasks = ref<Task[]>([
  {
    id: 1,
    title: 'Complete project documentation',
    description: 'Write comprehensive documentation for the admin panel project',
    status: 'In Progress',
    dueDate: '2024-04-15',
  },
  {
    id: 2,
    title: 'Review user management',
    description: 'Review and update user management functionality',
    status: 'Pending',
    dueDate: '2024-04-20',
  },
])

const showModal = ref(false)
const editingTask = ref<Task | null>(null)
const formData = ref({
  title: '',
  description: '',
  status: 'Pending',
  dueDate: '',
})

const openCreateModal = () => {
  editingTask.value = null
  formData.value = {
    title: '',
    description: '',
    status: 'Pending',
    dueDate: '',
  }
  showModal.value = true
}

const openEditModal = (task: Task) => {
  editingTask.value = task
  formData.value = { ...task }
  showModal.value = true
}

const closeModal = () => {
  showModal.value = false
  editingTask.value = null
}

const handleSubmit = () => {
  if (editingTask.value) {
    // Update existing task
    const index = tasks.value.findIndex((t) => t.id === editingTask.value?.id)
    if (index !== -1) {
      tasks.value[index] = { ...editingTask.value, ...formData.value }
    }
  } else {
    // Create new task
    const newTask: Task = {
      id: tasks.value.length + 1,
      ...formData.value,
    }
    tasks.value.push(newTask)
  }
  closeModal()
}

const deleteTask = (id: number) => {
  if (confirm('Are you sure you want to delete this task?')) {
    tasks.value = tasks.value.filter((task) => task.id !== id)
  }
}
</script>
