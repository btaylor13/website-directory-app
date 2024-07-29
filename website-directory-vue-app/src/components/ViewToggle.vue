<template>
  <select
    :value="modelValue"
    @change="$emit('update:modelValue', $event.target.value)"
    class="p-2 border rounded"
  >
    <option value="list">List View</option>
    <option value="grid">Grid View</option>
  </select>
</template>

<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>
