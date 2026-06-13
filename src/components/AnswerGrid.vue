<script setup lang="ts">
defineProps<{
  answers: { label: string; text: string }[]
  selected: string | null
  correct: string
  locked: boolean
}>()

const emit = defineEmits<{ select: [label: string] }>()

function stateClass(label: string, selected: string | null, correct: string, locked: boolean) {
  if (!locked) return ''
  if (label === correct) return 'correct'
  if (label === selected) return 'wrong'
  return 'dimmed'
}
</script>

<template>
  <div class="answer-grid">
    <button
      v-for="answer in answers"
      :key="answer.label"
      class="answer-btn"
      :class="stateClass(answer.label, selected, correct, locked)"
      :disabled="locked"
      @click="emit('select', answer.label)"
    >
      <span class="answer-label">{{ answer.label }}</span>
      <span class="answer-text">{{ answer.text }}</span>
      <span v-if="locked && answer.label === correct" class="answer-icon correct-icon">✓</span>
      <span v-else-if="locked && answer.label === selected && answer.label !== correct" class="answer-icon wrong-icon">✗</span>
    </button>
  </div>
</template>

<style scoped>
.answer-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.65rem;
}

@media (max-width: 520px) {
  .answer-grid { grid-template-columns: 1fr; }
}

.answer-btn {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  background: var(--surface);
  color: var(--text);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 0.9rem 1rem;
  font-size: 0.95rem;
  font-weight: 400;
  text-align: left;
  transition: all var(--transition);
  cursor: pointer;
  min-height: 60px;
}

.answer-btn:not(:disabled):hover {
  border-color: var(--border-hover);
  background: var(--surface-hover);
  transform: translateY(-1px);
}

.answer-btn:disabled { cursor: default; }

.answer-btn.correct {
  border-color: var(--green);
  background: var(--green-bg);
}

.answer-btn.wrong {
  border-color: var(--red);
  background: var(--red-bg);
}

.answer-btn.dimmed {
  background: transparent;
  border-color: var(--border);
  opacity: 0.35;
}

.answer-label {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1.75rem;
  height: 1.75rem;
  border-radius: 6px;
  background: var(--accent-dim);
  color: var(--accent);
  font-size: 0.8rem;
  font-weight: 700;
  flex-shrink: 0;
  transition: all var(--transition);
}

.answer-btn.correct .answer-label {
  background: rgba(74, 222, 128, 0.12);
  color: var(--green);
}

.answer-btn.wrong .answer-label,
.answer-btn.dimmed .answer-label {
  background: var(--border);
  color: var(--muted);
}

.answer-text {
  flex: 1;
  line-height: 1.4;
}

.answer-btn.correct .answer-text { color: var(--green); }
.answer-btn.wrong .answer-text   { color: var(--red); }
.answer-btn.dimmed .answer-text  { color: var(--muted); }

.answer-icon {
  font-size: 1rem;
  font-weight: 700;
  flex-shrink: 0;
}

.correct-icon { color: var(--green); }
.wrong-icon   { color: var(--red); }
</style>
