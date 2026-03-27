<template>
  <button 
    :class="[
      'inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-500 disabled:opacity-50 disabled:pointer-events-none ring-offset-bg-base',
      variants[variant],
      sizes[size],
      className
    ]"
    v-bind="$attrs"
  >
    <slot />
  </button>
</template>

<script setup lang="ts">
interface Props {
  variant?: 'default' | 'outline' | 'ghost' | 'link';
  size?: 'default' | 'sm' | 'lg' | 'icon';
  className?: string;
}

withDefaults(defineProps<Props>(), {
  variant: 'default',
  size: 'default',
  className: '',
});

const variants = {
  default: 'bg-primary-600 text-white hover:bg-primary-700',
  outline: 'border border-primary-500/30 hover:bg-primary-500/10 text-primary-400',
  ghost: 'hover:bg-bg-surface-hover text-text-base',
  link: 'underline-offset-4 hover:underline text-primary-500',
};

const sizes = {
  default: 'h-10 py-2 px-4',
  sm: 'h-9 px-3 rounded-md',
  lg: 'h-11 px-8 rounded-md',
  icon: 'h-10 w-10',
};
</script>
