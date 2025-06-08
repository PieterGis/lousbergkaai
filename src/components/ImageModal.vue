<template>
    <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-75" @click="closeModal">
      <div class="relative max-w-4xl max-h-[90vh] mx-4" @click.stop>
        <img :src="image" :alt="alt" class="max-w-full max-h-[90vh] object-contain rounded-2xl" />
        
        <!-- Navigation buttons -->
        <button 
          v-if="has_multiple_images" 
          @click="previousImage" 
          class="absolute left-4 top-1/2 -translate-y-1/2 text-white text-2xl hover:text-gray-300 bg-amber-500 rounded-full p-2 size-10 flex items-center justify-center"
        >
          <i class="fas fa-chevron-left"></i>
        </button>
        
        <button 
          v-if="has_multiple_images" 
          @click="nextImage" 
          class="absolute right-4 top-1/2 -translate-y-1/2 text-white text-2xl hover:text-gray-300 bg-amber-500 rounded-full p-2 size-10 flex items-center justify-center"
        >
          <i class="fas fa-chevron-right"></i>
        </button>
        
        <button @click="closeModal" class="absolute top-4 right-4 text-white text-2xl hover:text-gray-300 bg-amber-500 rounded-full p-2 size-10 flex items-center justify-center">
          <i class="fas fa-times"></i>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits, computed, onMounted, onUnmounted } from 'vue'
  
  const props = defineProps({
    isOpen: Boolean,
    image: String,
    alt: String,
    currentIndex: Number,
    totalImages: Number
  })
  
  const emit = defineEmits(['close', 'previous', 'next'])
  
  const has_multiple_images = computed(() => props.totalImages > 1)
  
  const closeModal = () => {
    emit('close')
  }
  
  const previousImage = () => {
    emit('previous')
  }
  
  const nextImage = () => {
    emit('next')
  }
  
  // Handle keyboard navigation
  const handleKeydown = (e) => {
    if (!props.isOpen) return
    
    if (e.key === 'ArrowLeft' && has_multiple_images.value) {
      previousImage()
    } else if (e.key === 'ArrowRight' && has_multiple_images.value) {
      nextImage()
    } else if (e.key === 'Escape') {
      closeModal()
    }
  }
  
  // Add keyboard event listener
  onMounted(() => {
    window.addEventListener('keydown', handleKeydown)
  })
  
  onUnmounted(() => {
    window.removeEventListener('keydown', handleKeydown)
  })
  </script>