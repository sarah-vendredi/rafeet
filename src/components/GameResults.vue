<script setup lang="ts">
import { computed } from 'vue'
import { rounds } from '../data/rounds'

const props = defineProps<{
  score: number
  total: number
  results: { roundId: number; correct: boolean }[]
}>()

defineEmits<{ replay: [] }>()

const percent = computed(() => Math.round((props.score / props.total) * 100))

const message = computed(() => {
  if (percent.value === 100) return 'Tu connais les pieds de Raphaël mieux que lui-même.'
  if (percent.value >= 80)  return 'Sérieusement impressionnant. Tu surveilles ses pieds ?'
  if (percent.value >= 60)  return 'Pas mal. Les pieds de Raphaël ont des secrets.'
  if (percent.value >= 40)  return 'C\'était compliqué. Les pieds de Raphaël restent mystérieux.'
  return 'Les pieds de Raphaël t\'ont totalement eu.'
})

function getRound(id: number) {
  return rounds.find(r => r.id === id)
}
</script>

<template>
  <div class="results">
    <div class="results-inner">

      <div class="results-brand">
        <span class="brand-ra">ra</span><span class="brand-feet">feet</span>
      </div>

      <div class="score-block">
        <div class="score-number">{{ score }}<span class="score-denom">/{{ total }}</span></div>
        <p class="score-msg">{{ message }}</p>
      </div>

      <div class="recap-list">
        <div
          v-for="result in results"
          :key="result.roundId"
          class="recap-item"
          :class="result.correct ? 'correct' : 'wrong'"
        >
          <span class="recap-icon">{{ result.correct ? '✓' : '✗' }}</span>
          <span class="recap-text">
            <em>{{ getRound(result.roundId)?.question }}</em>
          </span>
        </div>
      </div>

      <button class="btn-replay" @click="$emit('replay')">
        rejouer →
      </button>

    </div>
  </div>
</template>

<style scoped>
.results {
  flex: 1;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  min-height: 100vh;
  padding: 3rem 1.5rem;
}

.results-inner {
  max-width: 580px;
  width: 100%;
  text-align: center;
  animation: fadeIn 0.4s ease both;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.results-brand {
  font-size: 1.4rem;
  font-weight: 700;
  letter-spacing: -0.03em;
  user-select: none;
}

.brand-ra   { color: var(--muted); }
.brand-feet { color: var(--accent); }

.score-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
}

.score-number {
  font-size: clamp(5rem, 18vw, 9rem);
  font-weight: 700;
  letter-spacing: -0.05em;
  line-height: 1;
  color: var(--accent);
}

.score-denom {
  font-size: 0.45em;
  color: var(--muted);
  font-weight: 500;
}

.score-msg {
  font-size: 1rem;
  color: var(--muted-light);
  line-height: 1.6;
}

.recap-list {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  text-align: left;
  max-height: 340px;
  overflow-y: auto;
}

.recap-item {
  display: flex;
  align-items: flex-start;
  gap: 0.65rem;
  padding: 0.55rem 0.85rem;
  border-radius: 10px;
  font-size: 0.83rem;
  line-height: 1.4;
  border: 1px solid var(--border);
}

.recap-item.correct {
  background: var(--green-bg);
  border-color: rgba(74, 222, 128, 0.2);
}

.recap-item.wrong {
  background: var(--red-bg);
  border-color: rgba(248, 113, 113, 0.2);
}

.recap-icon {
  font-size: 0.85rem;
  font-weight: 700;
  flex-shrink: 0;
  margin-top: 0.05em;
}

.recap-item.correct .recap-icon { color: var(--green); }
.recap-item.wrong   .recap-icon { color: var(--red); }

.recap-text em {
  color: var(--muted-light);
  font-style: normal;
}

.btn-replay {
  background: transparent;
  color: var(--text);
  border: 1px solid var(--border-hover);
  font-size: 0.95rem;
  font-weight: 600;
  padding: 0.75em 2.5em;
  border-radius: var(--radius-pill);
  transition: all var(--transition);
}

.btn-replay:hover {
  border-color: var(--accent);
  color: var(--accent);
  background: var(--accent-dim);
}
</style>
