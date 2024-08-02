<template>
  <div class="wrapper">
    <BaseInput v-model="inputValue" label="Десятичне число" />
    <div class="terminal">
      Введене значення:
      <span v-if="inputValue.length" class="result">{{ inputValue }}</span>
      <span v-else class="empty">—</span>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import BaseInput from '@/components/BaseInput.vue'

const inputValue = ref('')
</script>

<style lang="scss">
.wrapper {
  @apply p-6 w-96 mx-auto cursor-default;
}
.terminal {
  @apply block mb-2 text-sm font-medium text-gray-900 dark:text-white mt-4 cursor-default;
}
.result {
  @apply text-green-700 font-normal inline cursor-text;
}
.empty {
  @apply text-gray-700 font-normal inline cursor-default;
}
</style>
