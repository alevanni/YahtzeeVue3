<script setup>
import ThrownDice from './components/ThrownDice.vue'
import ScoreTable from './components/ScoreTable.vue'
import { ref, reactive } from 'vue'

const count = reactive({
  1: 0,
  2: 0,
  3: 0,
  4: 0,
  5: 0,
  6: 0
})
</script>

<template>
  <div>
    <ThrownDice v-model:count="count"/>
    <ScoreTable v-model:count="count"/>
    <p>{{ count }}</p>
  </div>
  
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
