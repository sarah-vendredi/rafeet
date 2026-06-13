<script setup lang="ts">
defineEmits<{ play: [] }>()
</script>

<template>
  <div class="intro">
    <div class="intro-inner">
      <div class="wordmark">
        <span class="word-ra">ra</span><span class="word-feet">feet</span>
      </div>
      <p class="intro-sub">
        23 vidéos. Les pieds de Raphaël.<br />
        Sauras-tu deviner ce qu'ils font ?
      </p>
      <button class="btn-play" @click="$emit('play')">
        jouer →
      </button>
      <p class="copyright">fait pas Sarah</p>
    </div>
  </div>
</template>

<style scoped>
.intro {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 2rem;
}

.intro-inner {
  text-align: center;
  animation: fadeIn 0.5s ease both;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.wordmark {
  font-size: clamp(5rem, 18vw, 11rem);
  font-weight: 700;
  letter-spacing: -0.04em;
  line-height: 1;
  user-select: none;
}

.word-ra {
  color: var(--muted-light);
}

.word-feet {
  color: var(--accent);
}

.intro-sub {
  font-size: 1rem;
  color: var(--muted);
  line-height: 1.9;
  font-weight: 400;
}

.btn-play {
  display: inline-flex;
  align-items: center;
  background: var(--accent);
  color: var(--bg);
  font-size: 1rem;
  font-weight: 700;
  letter-spacing: 0.01em;
  padding: 0.8em 2.6em;
  border-radius: var(--radius-pill);
  transition: all var(--transition);
}

.btn-play:hover {
  background: #93c5fd;
  transform: translateY(-2px);
  box-shadow: 0 8px 24px var(--accent-glow);
}

.copyright {
  font-size: 0.7rem;
  color: var(--muted);
  opacity: 0.5;
}
</style>
