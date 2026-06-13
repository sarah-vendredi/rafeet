<script setup lang="ts">
import { ref, onMounted } from 'vue'

defineProps<{ videoSrc: string }>()

const videoEl = ref<HTMLVideoElement | null>(null)
const showFlash = ref(true)

onMounted(() => {
  setTimeout(() => {
    showFlash.value = false
    videoEl.value?.play()
  }, 400)
})
</script>

<template>
  <div class="reveal-wrap">
    <Transition name="flash">
      <div v-if="showFlash" class="flash-overlay" />
    </Transition>
    <video
      ref="videoEl"
      class="reveal-video"
      :src="videoSrc"
      controls
      playsinline
      preload="auto"
    />
  </div>
</template>

<style scoped>
.reveal-wrap {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: var(--radius);
  overflow: hidden;
  border: 1px solid var(--border-hover);
}

.reveal-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.flash-overlay {
  position: absolute;
  inset: 0;
  background: white;
  z-index: 10;
  pointer-events: none;
}

.flash-enter-active { animation: flashWhite 0.5s ease forwards; }
.flash-leave-active { transition: opacity 0.15s; }
.flash-leave-to    { opacity: 0; }
</style>
