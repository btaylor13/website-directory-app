<template>
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
    <div v-for="website in websites" :key="website.url" class="border p-4 rounded shadow">
      <h3 class="font-bold mb-2">{{ website.url }}</h3>
      <p>WP Version: {{ website.wpVersion }}</p>
    </div>
  </div>
</template>

<script setup>
defineProps({
  websites: {
    type: Array,
    required: true
  }
})
</script>
