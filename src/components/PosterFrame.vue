<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'

const props = defineProps<{
  videoSrc: string
  posterTime?: number
}>()

const videoEl = ref<HTMLVideoElement | null>(null)

function seekToPoster() {
  if (!videoEl.value) return
  videoEl.value.currentTime = props.posterTime ?? 1
}

onMounted(() => {
  const el = videoEl.value
  if (!el) return
  if (el.readyState >= 1) {
    seekToPoster()
  } else {
    el.addEventListener('loadedmetadata', seekToPoster, { once: true })
  }
})

watch(() => props.videoSrc, () => {
  if (videoEl.value) {
    videoEl.value.addEventListener('loadedmetadata', seekToPoster, { once: true })
    videoEl.value.load()
  }
})
</script>

<template>
  <div class="poster-wrap">
    <video
      ref="videoEl"
      class="poster-video"
      :src="videoSrc"
      preload="metadata"
      muted
      playsinline
    />
    <div class="poster-overlay" />
  </div>
</template>

<style scoped>
.poster-wrap {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: var(--radius);
  overflow: hidden;
  border: 1px solid var(--border);
}

.poster-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.poster-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 55%,
    rgba(9, 9, 11, 0.55) 100%
  );
}
</style>
