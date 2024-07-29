<template>
  <div class="container mx-auto px-4 py-8">
    <SearchBar @search="handleSearch" />

    <div class="flex justify-between my-4">
      <FilterDropdown @filter="applyFilters" />
      <select v-model="viewMode" class="p-2 border rounded">
        <option value="list">List View</option>
        <option value="grid">Grid View</option>
      </select>
    </div>

    <component :is="currentView" :websites="paginatedWebsites" @sort="handleSort" />

    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @page-change="handlePageChange"
    />
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import SearchBar from '../components/SearchBar.vue'
import FilterDropdown from '../components/FilterDropdown.vue'
import WebsiteList from '../components/WebsiteList.vue'
import WebsiteGrid from '../components/WebsiteGrid.vue'
import Pagination from '../components/Pagination.vue'

import axios from 'axios'

// Mock data - replace with API call in real implementation
const allWebsites = ref([
  { url: 'example1.com', wpVersion: '6.0' },
  { url: 'example2.com', wpVersion: '5.9' }
  // ... more websites
])

const searchQuery = ref('')
const selectedFilters = ref([])
const viewMode = ref('list')
const currentPage = ref(1)
const itemsPerPage = 10

const filteredWebsites = computed(() => {
  return allWebsites.value.filter((website) => {
    const matchesSearch = website.url.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesFilter =
      selectedFilters.value.length === 0 ||
      selectedFilters.value.some((filter) => {
        if (filter === 'earlier') {
          return parseFloat(website.wpVersion) < 5.7
        }
        return parseFloat(website.wpVersion) >= parseFloat(filter)
      })
    return matchesSearch && matchesFilter
  })
})

const paginatedWebsites = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return filteredWebsites.value.slice(start, end)
})

const totalPages = computed(() => {
  return Math.ceil(filteredWebsites.value.length / itemsPerPage)
})

const currentView = computed(() => {
  return viewMode.value === 'list' ? WebsiteList : WebsiteGrid
})

const handleSearch = (query) => {
  searchQuery.value = query
  currentPage.value = 1
}

const applyFilters = (filters) => {
  selectedFilters.value = filters
  currentPage.value = 1
}

const handleSort = ({ key, order }) => {
  allWebsites.value.sort((a, b) => {
    let modifier = order === 'asc' ? 1 : -1
    if (a[key] < b[key]) return -1 * modifier
    if (a[key] > b[key]) return 1 * modifier
    return 0
  })
}

const handlePageChange = (page) => {
  currentPage.value = page
}

onMounted(async () => {
  const response = await axios.get('/api/websites')
  allWebsites.value = await response.json()
})
</script>
