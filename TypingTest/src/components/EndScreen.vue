<script setup>
import ConfettiGenerator from 'confetti-js';
import { onMounted, onUnmounted } from 'vue';

const props = defineProps(['setScreen']);

const randomNumber = Math.floor(Math.random() * 7) + 1;
const phrases = ['Good Job!', 'Nice Work!', 'Woo-Hoo!', 'Keep it up!', "You're doing great!"];

let confetti;

onMounted(() => {
  const settings = { max: 300, rotate: true };
  confetti = new ConfettiGenerator(settings);
  confetti.render();
});

onUnmounted(() => {
  confetti.clear();
});
</script>

<template>
  <div class="">
    <canvas id="confetti-holder"></canvas>
    <div class="holder">
      <h1>{{ phrases[Math.floor(Math.random() * phrases.length)] }}</h1>
      <img :src="'src/assets/images/Congrats' + randomNumber + '.gif'" alt="" />
      <button @click="props.setScreen('type')">Try Again</button>
    </div>
  </div>
</template>

<style scoped>
.holder {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
img {
  margin: 30px;
}
</style>
