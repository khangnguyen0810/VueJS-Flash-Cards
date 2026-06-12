<script setup lang="ts">
import { ref, computed } from 'vue'
import type { Flashcard } from './types/Flashcard'
import FlashcardComponent from './components/Flashcard.vue'

const cards = ref<Flashcard[]>([
  {
    id: 1,
    category: 'Vue.js Basics',
    question: 'What is the purpose of ref() in Vue 3?',
    answer: 'It takes an inner value and returns a reactive and mutable ref object. The object has a single property .value that points to the inner value.',
    isFlipped: false
  },
  {
    id: 2,
    category: 'Tailwind CSS',
    question: 'What does utility-first mean?',
    answer: 'Instead of writing traditional custom scoped CSS rules, you style elements by applying pre-existing, single-purpose low-level utility classes directly in HTML/templates.',
    isFlipped: false
  },
  {
    id: 3,
    category: 'TypeScript',
    question: 'What is the difference between an Interface and a Type alias?',
    answer: 'Interfaces are primarily used to define object shapes and support extending/merging declarations. Types can define primitives, unions, and intersections.',
    isFlipped: false
  }
])

const currentIndex = ref(0)

const currentCard = computed(() => cards.value[currentIndex.value])

const handleToggleFlip = (id: number) => {
  const targetCard = cards.value.find(c => c.id === id)
  if (targetCard) {
    targetCard.isFlipped = !targetCard.isFlipped
  }
}

const nextCard = () => {
  if (currentCard.value) currentCard.value.isFlipped = false // Reset flip status
  if (currentIndex.value < cards.value.length - 1) {
    currentIndex.value++
  }
}

const prevCard = () => {
  if (currentCard.value) currentCard.value.isFlipped = false // Reset flip status
  if (currentIndex.value > 0) {
    currentIndex.value--
  }
}
</script>

<template>
  <main class="min-h-screen bg-slate-50 flex flex-col items-center justify-center p-6 antialiased">
    <div class="max-w-md w-full flex flex-col items-center gap-8">
      
      <header class="text-center">
        <h1 class="text-3xl font-extrabold tracking-tight text-slate-900">Study Deck</h1>
        <p class="text-sm text-slate-500 mt-1">Mastering Frontend Reactivity</p>
      </header>

      <div v-if="currentCard">
        <FlashcardComponent 
          :card="currentCard" 
          @toggle-flip="handleToggleFlip"
        />
      </div>

      <div class="flex items-center gap-6">
        <button 
          @click="prevCard" 
          :disabled="currentIndex === 0"
          class="px-4 py-2 text-sm font-medium rounded-lg bg-white border border-slate-200 text-slate-700 shadow-sm hover:bg-slate-50 disabled:opacity-40 disabled:cursor-not-allowed transition-colors"
        >
          Previous
        </button>
        
        <span class="text-sm font-medium text-slate-500">
          {{ currentIndex + 1 }} / {{ cards.length }}
        </span>
        <button 
          @click="nextCard" 
          :disabled="currentIndex === cards.length - 1"
          class="px-4 py-2 text-sm font-medium rounded-lg bg-indigo-600 text-white shadow-md shadow-indigo-600/10 hover:bg-indigo-700 disabled:opacity-40 disabled:cursor-not-allowed transition-colors"
        >
          Next Card
        </button>
      </div>

    </div>
  </main>
</template>