<script setup lang="ts">
// 1. Import the interface you created
import type { Flashcard } from '../types/Flashcard'

// 2. Define the incoming Props using compile-time TypeScript annotations
defineProps<{
  card: Flashcard
}>()

// 3. Define the custom event to notify the parent when a card is clicked
const emit = defineEmits<{
  (e: 'toggle-flip', id: number): void
}>()
</script>

<template>
  <div 
    @click="emit('toggle-flip', card.id)"
    class="w-80 h-auto [perspective:1000px] cursor-pointer"
  >
    <div 
      class="relative w-full h-full transition-transform duration-500 [transform-style:preserve-3d]"
      :class="{ '[transform:rotateY(180deg)]': card.isFlipped }"
    >
      
      <div class="absolute inset-0 w-full h-full p-6 flex flex-col justify-between rounded-2xl bg-white border border-slate-100 shadow-xl shadow-slate-200/50 [backface-visibility:hidden]">
        <div class="flex justify-between items-center">
          <span class="text-xs font-bold tracking-wider text-indigo-500 uppercase bg-indigo-50 px-2.5 py-1 rounded-md">
            {{ card.category }}
          </span>
          <span class="text-xs text-slate-400">#{{ card.id }}</span>
        </div>
        <h2 class="text-xl font-semibold text-slate-800 text-center my-auto px-2">
          {{ card.question }}
        </h2>
        <p class="text-[10px] tracking-wide text-slate-400 text-center uppercase animate-pulse">
          Click to flip
        </p>
      </div>

      <div class="w-full h-full p-6 flex flex-col justify-between rounded-2xl bg-indigo-600 text-white shadow-xl shadow-indigo-500/30 [backface-visibility:hidden] [transform:rotateY(180deg)]">
        <div class="flex justify-between items-center border-b border-indigo-500 pb-2">
          <span class="text-xs font-bold tracking-wider uppercase opacity-75">Answer</span>
          <span class="text-xs opacity-50">#{{ card.id }}</span>
        </div>
        <p class="text-lg font-medium text-center my-auto px-2 leading-relaxed">
          {{ card.answer }}
        </p>
        <p class="text-[10px] tracking-wide text-indigo-200 text-center uppercase">
          Click to see question
        </p>
      </div>

    </div>
  </div>
</template>