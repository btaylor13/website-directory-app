<template>
    <div class="filter-dropdown relative">
      <button
        @click="isOpen = !isOpen"
        class="p-2 border rounded flex items-center"
      >
        <span class="mr-2">Filters</span>
        <svg v-if="isOpen"></svg>
        <svg v-else></svg>
      </button>
      <div v-if="isOpen" class="absolute left-0 mt-2 w-64 bg-white border rounded shadow-lg p-4">
        <h3 class="font-bold mb-2">WordPress Version</h3>
        <div v-for="version in wpVersions" :key="version.value" class="mb-2">
          <label class="flex items-center">
            <input
              type="checkbox"
              v-model="selectedVersions"
              :value="version.value"
              class="mr-2"
            />
            {{ version.label }}
          </label>
        </div>
        <button @click="clearFilters" class="mt-4 p-2 bg-gray-200 rounded w-full">
          Clear All
        </button>
      </div>
    </div>
</template>


<script setup>
import { ref, watch } from 'vue'

const isOpen = ref(false)
const selectedVersions = ref([])

const wpVersions = [
  { label: '6.6 and up', value: '6.6' },
  { label: '6.5 and up', value: '6.5' },
  { label: '6.4 and up', value: '6.4' },
  { label: '6.3 and up', value: '6.3' },
  { label: 'Earlier versions', value: 'earlier' }
]

const emit = defineEmits(['filter'])

watch(selectedVersions, (newVersions) => {
  emit('filter', newVersions)
})

const clearFilters = () => {
  selectedVersions.value = []
}
</script>