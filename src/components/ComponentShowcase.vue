<template>
  <div class="border border-bg-surface rounded-lg overflow-hidden mb-8 bg-[#0d0d0f]">
    <div class="flex items-center justify-between px-4 py-3 border-b border-bg-surface bg-bg-surface/50">
      <h3 class="text-lg font-medium text-white">{{ title }}</h3>
      <div class="flex bg-bg-base rounded-md p-1">
        <button 
          @click="view = 'preview'" 
          :class="['px-3 py-1.5 text-sm rounded-sm transition-colors', view === 'preview' ? 'bg-bg-surface text-primary-400' : 'text-text-muted hover:text-white']"
        >
          Preview
        </button>
        <button 
          @click="view = 'code'" 
          :class="['px-3 py-1.5 text-sm rounded-sm transition-colors', view === 'code' ? 'bg-bg-surface text-primary-400' : 'text-text-muted hover:text-white']"
        >
          Code
        </button>
      </div>
    </div>
    
    <div class="p-0">
      <div v-show="view === 'preview'" class="p-8 flex items-center justify-center min-h-[200px] bg-gradient-to-br from-bg-base to-bg-surface">
        <slot name="preview" />
      </div>
      <div v-show="view === 'code'" class="p-0 bg-[#1e1e1e] overflow-x-auto text-sm">
        <slot name="code" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

defineProps<{
  title: string;
}>();

const view = ref<'preview' | 'code'>('preview');
</script>
