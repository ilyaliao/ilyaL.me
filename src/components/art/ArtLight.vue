<script setup lang="ts">
import { breakpointsTailwind, useBreakpoints, useDark } from '@vueuse/core'
import { computed } from 'vue'

const isDark = useDark()
const { isSmaller } = useBreakpoints(breakpointsTailwind)

const size = computed(() => isSmaller('md') ? 400 : 520)
const opacity = computed(() => isDark.value ? 0.5 : 0.75)
</script>

<template>
  <div id="glow-container">
    <div
      class="glow-effect glow-1"
      :style="{
        width: `${size * 1.5}px`,
        height: `${size * 1.5}px`,
        opacity,
      }"
    />
    <div
      class="glow-effect glow-2"
      :style="{
        width: `${size * 1.2}px`,
        height: `${size * 1.2}px`,
        opacity,
      }"
    />
  </div>
</template>

<style scoped>
#glow-container {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  z-index: -1;
  overflow: hidden;
  pointer-events: none;
}

.glow-effect {
  position: absolute;
  border-radius: 50%;
  transition: all 0.6s ease-in-out;
}

.glow-1 {
  top: 0;
  right: 0;
  transform: translate(30%, -30%);
  background: linear-gradient(145deg, rgba(3, 105, 161) 0%, rgba(56, 189, 248, 0.7) 100%);
  filter: blur(180px);
  animation: glow-fade-in-top-right 1s ease-out;
}

.glow-2 {
  top: 100%;
  left: 0%;
  transform: translate(-50%, -50%);
  background: linear-gradient(120deg, rgb(56, 189, 248) 0%, rgb(3, 105, 161) 50%, rgb(56 189 248) 100%);
  filter: blur(140px);
  animation: glow-fade-in-bottom-left 1s ease-out;
}

@keyframes glow-fade-in-top-right {
  0% {
    opacity: 0;
    transform: translate(30%, -30%) scale(0.5);
  }
  100% {
    opacity: v-bind(opacity);
    transform: translate(30%, -30%) scale(1);
  }
}

@keyframes glow-fade-in-bottom-left {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
  100% {
    opacity: v-bind(opacity);
    transform: translate(-50%, -50%) scale(1);
  }
}

:root.dark .glow-effect {
  filter: blur(160px);
}
</style>
