<template>
  <div class="flex justify-center items-center space-x-2 mt-4">
    <button
      @click="emit('page-change', currentPage - 1)"
      :disabled="currentPage === 1"
      class="p-2 border rounded disabled:opacity-50"
    >
      Previous
    </button>
    <span>Page {{ currentPage }} of {{ totalPages }}</span>
    <button
      @click="emit('page-change', currentPage + 1)"
      :disabled="currentPage === totalPages"
      class="p-2 border rounded disabled:opacity-50"
    >
      Next
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  currentPage: {
    type: Number,
    required: true
  },
  totalPages: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['page-change'])
</script>
