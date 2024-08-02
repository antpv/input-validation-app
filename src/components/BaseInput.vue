<template>
  <label class="label">
    {{ label }}
    <input
      :value="inputValue"
      @keydown="handleKeydown"
      @input="handleInput"
      type="text"
      class="input"
    />
  </label>
  <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  modelValue: {
    type: String,
    default: '',
  },
  label: {
    type: String,
    default: 'Input',
  },
});

const emit = defineEmits(['update:modelValue']);

const inputValue = ref(props.modelValue);
const previousValue = ref(props.modelValue);
const errorMessage = ref('');

const isInvalid = (value) => {
  const regex = /^\d*(\.\d{0,18})?$/;
  return !regex.test(value);
};

watch(
  () => props.modelValue, (newValue) => {
    if (isInvalid(newValue)) {
      inputValue.value = '';
      emit('update:modelValue', '');
    } else {
      inputValue.value = newValue;
    }
  },
  {
    immediate: true
  }
);

const handleKeydown = (event) => {
  previousValue.value = event.target.value;
};

const handleInput = (event) => {
  if (isInvalid(event.target.value)) {
    inputValue.value = event.target.value = previousValue.value;
    errorMessage.value = 'Лише числа й одна десяткова крапка, кількість десяткових знаків до 18';
  } else {
    emit('update:modelValue', event.target.value);
    errorMessage.value = '';
  }
};
</script>

<style lang="scss">
.input {
  @apply bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 mt-2;
}
.error-message {
  @apply mt-2 text-sm text-red-500 cursor-default;
}
.label {
  @apply block mb-2 text-sm font-medium text-gray-900 dark:text-white cursor-default;
}
</style>
