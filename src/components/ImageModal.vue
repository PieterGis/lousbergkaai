<template>
    <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-75" @click="closeModal">
      <div class="relative max-w-4xl max-h-[90vh] mx-4">
        <img :src="image" :alt="alt" class="max-w-full max-h-[90vh] object-contain" />
        <button @click="closeModal" class="absolute top-4 right-4 text-white text-2xl hover:text-gray-300 bg-amber-500 rounded-full p-2 size-10 flex items-center justify-center">
          <i class="fas fa-times"></i>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits } from 'vue'
  
  defineProps({
    isOpen: Boolean,
    image: String,
    alt: String
  })
  
  const emit = defineEmits(['close'])
  
  const closeModal = () => {
    emit('close')
  }
  </script>