<script setup lang="ts">
import { ref } from 'vue'
import type { Round } from '../data/rounds'
import PosterFrame from './PosterFrame.vue'
import QuestionBlock from './QuestionBlock.vue'
import AnswerGrid from './AnswerGrid.vue'
import VideoReveal from './VideoReveal.vue'

type RoundState = 'question' | 'answered' | 'revealing'

const props = defineProps<{
  round: Round
  roundNumber: number
  totalRounds: number
  score: number
  isLastRound: boolean
}>()

const emit = defineEmits<{
  roundComplete: [correct: boolean]
  next: []
}>()

const state = ref<RoundState>('question')
const selected = ref<string | null>(null)

function onSelect(label: string) {
  if (state.value !== 'question') return
  selected.value = label
  state.value = 'answered'
  emit('roundComplete', label === props.round.correctAnswer)
}

function reveal() {
  state.value = 'revealing'
}
</script>

<template>
  <div class="game-round">
    <!-- Progress bar -->
    <div class="progress-track">
      <div
        class="progress-fill"
        :style="{ width: `${(roundNumber / totalRounds) * 100}%` }"
      />
    </div>

    <!-- Header -->
    <header class="game-header">
      <span class="header-brand">
        <span class="brand-ra">ra</span><span class="brand-feet">feet</span>
      </span>
      <div class="header-right">
        <span class="header-score">{{ score }} pt{{ score !== 1 ? 's' : '' }}</span>
        <span class="header-sep">·</span>
        <span class="header-round">{{ roundNumber }}&thinsp;/&thinsp;{{ totalRounds }}</span>
      </div>
    </header>

    <main class="round-content">
      <!-- Question phase -->
      <template v-if="state !== 'revealing'">
        <div class="media-wrap">
          <PosterFrame :video-src="round.video" :poster-time="round.posterTime" />
        </div>

        <QuestionBlock
          :question="round.question"
          :round-number="roundNumber"
          :total-rounds="totalRounds"
        />

        <AnswerGrid
          :answers="round.answers"
          :selected="selected"
          :correct="round.correctAnswer"
          :locked="state === 'answered'"
          @select="onSelect"
        />

        <Transition name="slide-up">
          <div v-if="state === 'answered'" class="reveal-cta">
            <div class="feedback-text" :class="selected === round.correctAnswer ? 'good' : 'bad'">
              {{ selected === round.correctAnswer ? 'bravo 🎉' : 'raté 😬' }}
            </div>
            <button class="btn-reveal" @click="reveal">
              révéler la vidéo ▶
            </button>
          </div>
        </Transition>
      </template>

      <!-- Reveal phase -->
      <template v-else>
        <VideoReveal :video-src="round.video" />

        <div class="next-cta">
          <p class="score-summary">{{ score }} point{{ score !== 1 ? 's' : '' }} sur {{ totalRounds }}</p>
          <button class="btn-next" @click="emit('next')">
            {{ isLastRound ? 'voir les résultats →' : 'round suivant →' }}
          </button>
        </div>
      </template>
    </main>
  </div>
</template>

<style scoped>
.game-round {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* Progress */
.progress-track {
  height: 3px;
  background: var(--border);
  position: sticky;
  top: 0;
  z-index: 30;
}

.progress-fill {
  height: 100%;
  background: var(--accent);
  transition: width 0.4s ease;
}

/* Header */
.game-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.85rem 1.5rem;
  border-bottom: 1px solid var(--border);
  position: sticky;
  top: 3px;
  z-index: 20;
  background: rgba(9, 9, 11, 0.85);
  backdrop-filter: blur(12px);
}

.header-brand {
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: -0.03em;
  user-select: none;
}

.brand-ra   { color: var(--muted); }
.brand-feet { color: var(--accent); }

.header-right {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.85rem;
  color: var(--muted);
}

.header-score { color: var(--text); font-weight: 500; }
.header-sep   { color: var(--muted); }

/* Content */
.round-content {
  flex: 1;
  max-width: 760px;
  width: 100%;
  margin: 0 auto;
  padding: 1.5rem 1rem 2.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  animation: fadeIn 0.3s ease both;
}

.media-wrap {
  width: 100%;
}

/* Reveal CTA */
.reveal-cta {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.feedback-text {
  font-size: 1.25rem;
  font-weight: 700;
}

.feedback-text.good { color: var(--green); }
.feedback-text.bad  { color: var(--red); }

.btn-reveal {
  background: transparent;
  color: var(--text);
  border: 1px solid var(--border-hover);
  font-size: 0.95rem;
  font-weight: 600;
  padding: 0.7em 2.2em;
  border-radius: var(--radius-pill);
  transition: all var(--transition);
}

.btn-reveal:hover {
  border-color: var(--accent);
  color: var(--accent);
  background: var(--accent-dim);
}

/* Next CTA */
.next-cta {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  text-align: center;
}

.score-summary {
  font-size: 0.9rem;
  color: var(--muted);
}

.btn-next {
  background: var(--accent);
  color: var(--bg);
  font-size: 1rem;
  font-weight: 700;
  padding: 0.75em 2.5em;
  border-radius: var(--radius-pill);
  transition: all var(--transition);
}

.btn-next:hover {
  background: #93c5fd;
  transform: translateY(-2px);
  box-shadow: 0 8px 24px var(--accent-glow);
}

/* Transitions */
.slide-up-enter-active { transition: all 0.25s ease; }
.slide-up-enter-from  { opacity: 0; transform: translateY(12px); }
</style>
