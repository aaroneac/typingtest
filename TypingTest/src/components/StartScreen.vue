<script setup>
const props = defineProps(['setScreen']);
</script>

<template>
  <div class="body">
    <div class="image-holder">
      <img class="background-image" src="../assets/images/racebackground.jpg" alt="" />
    </div>
    <div class="holder">
      <h1>Welcome to the Typing Test!</h1>
      <button @click="props.setScreen('type')">Start Typing</button>
    </div>
  </div>
</template>

<style scoped>
.body {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.background-image {
  min-width: 100vw;
  max-width: 100vw;
  min-height: 100vh;
  max-height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  object-fit: fill;
}
.holder {
  background-color: rgba(0, 0, 0, 0.3);
  padding: 15px;
  border-radius: 5px;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
button {
  margin-top: 15px;
}
</style>
