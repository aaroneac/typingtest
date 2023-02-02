<script setup>
import StartScreen from './components/StartScreen.vue';
import TypeScreen from './components/TypeScreen.vue';
import EndScreen from './components/EndScreen.vue';
import { reactive } from 'vue';

const state = reactive({
  screen: 'start',
});

function setScreen(screen) {
  state.screen = screen;
}
</script>

<template>
  <div class="body">
    <div v-if="state.screen === 'start'">
      <StartScreen :setScreen="setScreen" />
    </div>
    <div v-else-if="state.screen === 'type'">
      <TypeScreen :setScreen="setScreen" />
    </div>
    <div v-else>
      <EndScreen :setScreen="setScreen" />
    </div>
  </div>
</template>

<style scoped></style>
