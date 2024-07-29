<template>
  <table class="w-full border-collapse">
    <thead>
      <tr class="bg-gray-100">
        <th @click="emitSort('url')" class="p-2 text-left cursor-pointer">
          Website Name
          <SortIndicator :active="sortKey === 'url'" :ascending="sortOrder === 'asc'" />
        </th>
        <th @click="emitSort('wpVersion')" class="p-2 text-left cursor-pointer">
          WP Version
          <SortIndicator :active="sortKey === 'wpVersion'" :ascending="sortOrder === 'asc'" />
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="website in sortedWebsites" :key="website.url" class="border-b">
        <td class="p-2">{{ website.url }}</td>
        <td class="p-2">{{ website.wpVersion }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref, computed } from 'vue'
import SortIndicator from './SortIndicator.vue'

const props = defineProps({
  websites: Array
})

const sortKey = ref('url')
const sortOrder = ref('asc')

const emit = defineEmits(['sort'])

const emitSort = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortKey.value = key
    sortOrder.value = 'asc'
  }
  emit('sort', { key: sortKey.value, order: sortOrder.value })
}

const sortedWebsites = computed(() => {
  return [...props.websites].sort((a, b) => {
    let modifier = sortOrder.value === 'asc' ? 1 : -1
    if (a[sortKey.value] < b[sortKey.value]) return -1 * modifier
    if (a[sortKey.value] > b[sortKey.value]) return 1 * modifier
    return 0
  })
})
</script>
