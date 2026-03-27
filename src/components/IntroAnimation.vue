<template>
  <div v-if="isVisible" ref="introContainer" class="fixed inset-0 z-50 flex items-center justify-center bg-bg-base overflow-hidden">
    <div class="relative flex flex-col items-center justify-center">
      <div ref="logoText" class="text-6xl md:text-8xl font-black tracking-tighter text-white opacity-0 translate-y-12">
        <span class="text-white">OVER</span><span class="text-primary-500">THINKER</span>
      </div>
      
      <div ref="subText" class="mt-6 text-lg md:text-xl text-primary-300 font-mono tracking-widest opacity-0">
        System Initializing...
      </div>
      
      <!-- Decorative elements -->
      <div ref="circle1" class="absolute w-[40vw] h-[40vw] border-[1px] border-primary-500/20 rounded-full scale-0 opacity-0 pointer-events-none"></div>
      <div ref="circle2" class="absolute w-[60vw] h-[60vw] border-[1px] border-primary-400/10 rounded-full scale-0 opacity-0 pointer-events-none"></div>
      
      <div ref="progressBar" class="absolute bottom-[-40px] left-1/2 -translate-x-1/2 w-48 h-1 bg-bg-surface overflow-hidden rounded-full opacity-0">
        <div ref="progressFill" class="h-full bg-primary-500 w-0"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import gsap from 'gsap';

const isVisible = ref(true);
const introContainer = ref<HTMLElement | null>(null);
const logoText = ref<HTMLElement | null>(null);
const subText = ref<HTMLElement | null>(null);
const circle1 = ref<HTMLElement | null>(null);
const circle2 = ref<HTMLElement | null>(null);
const progressBar = ref<HTMLElement | null>(null);
const progressFill = ref<HTMLElement | null>(null);

onMounted(() => {
  // Check if we've already shown the intro in this session
  const hasSeenIntro = sessionStorage.getItem('hasSeenIntro');
  
  if (hasSeenIntro) {
    isVisible.value = false;
    document.body.classList.remove('no-scroll');
    return;
  }
  
  // Prevent scrolling during animation
  document.body.classList.add('no-scroll');
  
  const tl = gsap.timeline({
    onComplete: () => {
      isVisible.value = false;
      document.body.classList.remove('no-scroll');
      sessionStorage.setItem('hasSeenIntro', 'true');
    }
  });

  // 1. Initial reveal of logo text
  tl.to(logoText.value, {
    y: 0,
    opacity: 1,
    duration: 1.2,
    ease: "power4.out"
  })
  
  // 2. Expand decorative circles
  .to([circle1.value, circle2.value], {
    scale: 1,
    opacity: 1,
    duration: 1.5,
    stagger: 0.2,
    ease: "expo.out"
  }, "-=0.8")
  
  // 3. Show subtext and progress bar container
  .to([subText.value, progressBar.value], {
    opacity: 1,
    duration: 0.5,
    ease: "power2.out"
  }, "-=1")
  
  // 4. Simulate loading progress
  .to(progressFill.value, {
    width: "100%",
    duration: 1.5,
    ease: "power1.inOut",
    onUpdate: function() {
      if (subText.value) {
        const progress = Math.round(this.progress() * 100);
        if (progress < 30) subText.value.innerText = "Loading Modules...";
        else if (progress < 60) subText.value.innerText = "Compiling Assets...";
        else if (progress < 90) subText.value.innerText = "Starting Engine...";
        else subText.value.innerText = "Ready.";
      }
    }
  })
  
  // 5. Exit animation
  .to([circle1.value, circle2.value], {
    scale: 1.5,
    opacity: 0,
    duration: 0.8,
    ease: "power2.in"
  })
  .to(logoText.value, {
    scale: 1.1,
    opacity: 0,
    duration: 0.6,
    ease: "power2.in"
  }, "-=0.6")
  .to([subText.value, progressBar.value], {
    opacity: 0,
    duration: 0.4
  }, "-=0.4")
  .to(introContainer.value, {
    opacity: 0,
    duration: 0.5,
    ease: "power2.inOut"
  });
});
</script>
