<template>
    <div class="search-bar">
      <input
        v-model="searchQuery"
        @input="emitSearch"
        placeholder="Search websites..."
        class="p-2 border rounded-l"
      />
      <button @click="emitSearch" class="bg-blue-500 text-white p-2 rounded-r">Search</button>
    </div>
</template>

<script setup>
    import { ref, watch } from 'vue'

    const searchQuery = ref('')
    const emit = defineEmits(['search'])

    const emitSearch = () => {
        emit('search', searchQuery.value)
    }

    // Debounce search to avoid too many emissions
    watch(searchQuery, (newQuery) => {
    setTimeout(() => {
        emit('search', newQuery)
    }, 300)
    })
</script>