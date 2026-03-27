<template>
  <div class="fixed inset-0 z-[-1] pointer-events-none">
    <div class="absolute inset-0 bg-[#0d0d0f]"></div>
    
    <!-- 流动磨砂背景 -->
    <div class="absolute inset-0 opacity-30 mix-blend-screen overflow-hidden">
      <div class="absolute w-[800px] h-[800px] -top-[200px] -left-[200px] bg-primary-600/30 rounded-full blur-[100px] animate-blob"></div>
      <div class="absolute w-[800px] h-[800px] top-[20%] -right-[200px] bg-blue-600/30 rounded-full blur-[100px] animate-blob animation-delay-2000"></div>
      <div class="absolute w-[800px] h-[800px] -bottom-[200px] left-[20%] bg-purple-600/30 rounded-full blur-[100px] animate-blob animation-delay-4000"></div>
    </div>
    
    <!-- 噪点遮罩 -->
    <div class="absolute inset-0 bg-noise opacity-[0.03]"></div>
    
    <!-- 粒子特效 -->
    <vue-particles
      v-if="init"
      id="tsparticles"
      :options="options"
      class="absolute inset-0"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { loadSlim } from '@tsparticles/slim';

const init = ref(false);

onMounted(async () => {
  const { tsParticles } = await import('@tsparticles/engine');
  await loadSlim(tsParticles);
  init.value = true;
});

const options = {
  background: {
    color: {
      value: "transparent",
    },
  },
  fpsLimit: 60,
  interactivity: {
    events: {
      onHover: {
        enable: true,
        mode: "grab",
      },
    },
    modes: {
      grab: {
        distance: 140,
        links: {
          opacity: 0.5
        }
      }
    },
  },
  particles: {
    color: {
      value: ["#3b82f6", "#8b5cf6", "#60a5fa"], // 蓝色和紫色主题
    },
    links: {
      color: "#3b82f6",
      distance: 150,
      enable: true,
      opacity: 0.2,
      width: 1,
    },
    move: {
      direction: "none" as const,
      enable: true,
      outModes: {
        default: "bounce" as const,
      },
      random: true,
      speed: 0.8,
      straight: false,
    },
    number: {
      density: {
        enable: true,
        width: 800,
      },
      value: 60,
    },
    opacity: {
      value: 0.4,
      animation: {
        enable: true,
        speed: 1,
        minimumValue: 0.1,
      }
    },
    shape: {
      type: "circle",
    },
    size: {
      value: { min: 1, max: 3 },
      animation: {
        enable: true,
        speed: 2,
        minimumValue: 0.5,
      }
    },
  },
  detectRetina: true,
};
</script>

<style>
@keyframes blob {
  0% { transform: translate(0px, 0px) scale(1); }
  33% { transform: translate(30px, -50px) scale(1.1); }
  66% { transform: translate(-20px, 20px) scale(0.9); }
  100% { transform: translate(0px, 0px) scale(1); }
}

.animate-blob {
  animation: blob 15s infinite alternate;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

.animation-delay-4000 {
  animation-delay: 4s;
}

.bg-noise {
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
}
</style>
