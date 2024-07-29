<script setup>
import Header from './components/Header.vue'
import WebsiteDirectory from './views/WebsiteDirectory.vue'
</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <Header />
    <main class="container mx-auto px-4 py-8">
      <WebsiteDirectory />
    </main>
  </div>
</template>

<style scoped></style>
