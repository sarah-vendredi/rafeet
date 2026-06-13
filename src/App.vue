<script setup lang="ts">
import { ref } from 'vue'
import { rounds } from './data/rounds'
import type { Round } from './data/rounds'
import GameIntro from './components/GameIntro.vue'
import GameRound from './components/GameRound.vue'
import GameResults from './components/GameResults.vue'

type Screen = 'intro' | 'game' | 'results'

const screen = ref<Screen>('intro')
const currentRoundIndex = ref(0)
const score = ref(0)
const results = ref<{ roundId: number; correct: boolean }[]>([])
const shuffledRounds = ref<Round[]>([])

function shuffle<T>(arr: T[]): T[] {
  const a = [...arr]
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[a[i], a[j]] = [a[j], a[i]]
  }
  return a
}

function startGame() {
  shuffledRounds.value = shuffle(rounds)
  currentRoundIndex.value = 0
  score.value = 0
  results.value = []
  screen.value = 'game'
}

function onRoundComplete(correct: boolean) {
  results.value.push({ roundId: shuffledRounds.value[currentRoundIndex.value].id, correct })
  if (correct) score.value++
}

function nextRound() {
  if (currentRoundIndex.value < shuffledRounds.value.length - 1) {
    currentRoundIndex.value++
  } else {
    screen.value = 'results'
  }
}
</script>

<template>
  <GameIntro v-if="screen === 'intro'" @play="startGame" />

  <GameRound
    v-else-if="screen === 'game'"
    :key="currentRoundIndex"
    :round="shuffledRounds[currentRoundIndex]"
    :round-number="currentRoundIndex + 1"
    :total-rounds="shuffledRounds.length"
    :score="score"
    :is-last-round="currentRoundIndex === rounds.length - 1"
    @round-complete="onRoundComplete"
    @next="nextRound"
  />

  <GameResults
    v-else
    :score="score"
    :total="rounds.length"
    :results="results"
    @replay="startGame"
  />
</template>
