<script setup>
import ThrownDice from './components/ThrownDice.vue'
import ScoreTable from './components/ScoreTable.vue'
import { ref, reactive } from 'vue'

const dice = reactive([0, 0, 0, 0, 0]);

const updateDice = (data) => {
  for (let i=0; i<dice.length; i++) {
    dice[i] = data[i];
  }
  
}
</script>

<template>
  <div>
    <ThrownDice v-model:dice="dice"  @throwDice="updateDice"/>
    <ScoreTable v-model:dice="dice" />
   
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