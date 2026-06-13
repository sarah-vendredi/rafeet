<script setup lang="ts">
defineProps<{
  question: string
  roundNumber: number
  totalRounds: number
}>()
</script>

<template>
  <div class="question-block">
    <h2 class="question-text">{{ question }}</h2>
  </div>
</template>

<style scoped>
.question-block {
  text-align: center;
  padding: 0.25rem 0;
}

.question-text {
  font-size: clamp(1.15rem, 3.5vw, 1.6rem);
  font-weight: 500;
  color: var(--text);
  line-height: 1.45;
}
</style>
