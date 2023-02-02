<script setup>
import { onMounted, onUnmounted, reactive } from 'vue';

const props = defineProps(['setScreen']);

const state = reactive({
  letterIndex: 0,
  correctTypingStrokes: [],
  timeLeft: 180,
  car1Left: 0,
  car2Left: 0,
});

const passages = [
  "Look at my favourite toys. This is my car. I play with my car in the garden. This is my puzzle. It's got a picture of a cat. I do puzzles with my dad. This is my doll. Her name's Penny. Look at my favourite toys. This is my ball. I play ball with my sister. Lucy. This is my boat. I play with my boat in the bath. This is my teddy. His name is Ted.",
  'Anna has a new bike. It is a shiny blue colour. Annas brother Tim also has a bike. Tims bike is silver. When Anna and Tim ride their bikes they always wear helmets. Thats because they know about road safety.',
  'These fish have names. This is Finny. Finny has beautiful long fins that help her swim fast. This is Tayla. Taylas big tail moves from side to side and it helps her to go this way or that way. This is Igor. Igor has great big eyes. Igors great big eyes help him to see where he is going and they also help him to see big scary fish',
  'Ben is a little boy. He is 5 years old. Ben likes to play with his truck and toy cars. The cars can go very fast. When Ben plays with his cars he makes a sound like this Vroom.',
  'Ben has a sister. Her name is Sally and she is 4 years old. Sally has a doll. The name of Sallys doll is Tammy. Sally likes to dress Tammy in pretty clothes.',
  'Sam has a yellow ball. It is a bouncy. bouncy ball. When Sam pats his ball to the ground it bounces right back up to his hand. If Sam pats the ball softly it only bounces up a little bit. If he pats it hard it bounces up high',
  'Mike has a ball too. Mikes ball is small and blue. Mike likes to bounce his ball with both hands. If Mikes ball hits a little stone or stick it bounces away from him. Then he runs and gets it and starts bouncing again. ',
  'One day in the forest a fox chased after a rabbit. The rabbit hid behind a tree and the fox kept running.',
  'Olly was always happy but Freda was always sad. One day Olly said to Freda. Why do you look so sad. Freda said Im sad because you have lots of arms and I dont. Oh said Olly. dont be sad just be happy with what you do have nice fins and a tail to help you swim well.',
  'Patch got a bone for Christmas. He found it in the morning in a Christmas stocking right outside his kennel. This is the first time Patch has got a bone for Christmas. Every other year hes got a bowl of doggy sweets.',
  'Long ago. far away in the outback. lived Jeddi. a young aboriginal boy. Jeddi and his father were out hunting one day when they saw a wallaby lying on the ground.',
  'Anna has a little pup. The pups name is Rollo. Rollo is white with brown spots. Anna gave Rollo a toy bone to play with. When Anna throws the bone Rollo runs to get it as fast as he can.',
  'Mr Smith lives in a great big blue house. On the bottom there are three steps and a door. There are five windows at the front of the house.',
  'Wanda the wombat has small eyes. pointy ears and a dark flat nose. She walks very slowly on her short stumpy legs. Wanda lives in a tunnel. She dug the tunnel with her very strong paws.',
  'Mrs Brown has a new car. It is a nice red colour. The car has four doors two at the front and two at the back. Mrs Brown will now be able to drive her three children to school.',
  'Greg likes to help his mom make cookies. Chocolate chip cookies are his favorite. His mom likes them too. He adds the chocolate chips to the batter. Then he stirs them in. He forms the batter into balls and places them on a baking sheet. His mom places the baking sheet into the oven. When the cookies are in the oven Greg licks the last bits of batter from the bowl. He can hardly wait for the cookies to be ready.',
  'Brad loves to play video games. His favorites are Disney games. Brad got home from school one day and his mom asked him if he had homework. He did but he thought about saying no because he wanted to play his games.',
  'Emma has a new bicycle. It is bright pink and shiny. It was a gift from her uncle. He hid it behind a bush to surprise her. When Emma looked behind the bush and saw the bicycle she jumped for joy. It was just what she wanted. She gave her uncle a big hug. She loves her new bicycle and she loves her uncle.',
  'It is getting cold. Bear needs to get ready for winter. First he eats a lot. Next he finds a den. Then he fills the den with leaves so he will stay warm. Last he eats even more. Is Bear ready for winter. Yes he is. Winter is here!',
  'My family just adopted a puppy and a kitten from the animal shelter. They were so young they did not even have names yet. We had to think of good names for them. The puppy likes to jump up. The kitten likes to curl up in our laps.',
];

const passage = passages[Math.floor(Math.random() * passages.length)];

const interval = setInterval(() => {
  state.timeLeft = state.timeLeft - 1;
  const t = (state.timeLeft / 180) * 100;
  state.car1Left = 100 - t;
  if (state.timeLeft < 1) {
    finished();
  }
}, 1000);

function keyDownListener(e) {
  if (e.key === 'Backspace') {
    state.letterIndex = state.letterIndex > 0 ? state.letterIndex - 1 : 0;
    state.correctTypingStrokes.pop();
  }
}

function keyPressListener(e) {
  if (String.fromCharCode(e.keyCode).toLowerCase() === passage[state.letterIndex].toLowerCase()) {
    state.correctTypingStrokes.push(true);
    const t = 100 / passage.length;
    state.car2Left = state.car2Left + t;
  } else {
    state.correctTypingStrokes.push(false);
  }
  state.letterIndex++;

  if (state.letterIndex === passage.length) {
    finished();
  }
}

onMounted(() => {
  window.addEventListener('keydown', keyDownListener, false);
  window.addEventListener('keypress', keyPressListener, false);
});

onUnmounted(() => {
  clearInterval(interval);
  window.removeEventListener('keydown', keyDownListener, false);
  window.removeEventListener('keypress', keyPressListener, false);
});

function finished() {
  props.setScreen('end');
}

function generateClasses(index) {
  let classString = '';

  if (state.letterIndex === index) {
    classString += ' selectedLetter ';
  }

  if (state.correctTypingStrokes[index]) {
    classString += ' correctLetter ';
  } else if (state.correctTypingStrokes[index] === false) {
    classString += ' incorrectLetter ';
  }

  return classString;
}
</script>

<template>
  <div class="div">
    <div class="car-background"></div>
    <img src="../assets/images/car1.png" alt="" class="car car1" :style="{ left: state.car1Left + '%' }" />
    <img src="../assets/images/car2.png" alt="" class="car car2" :style="{ left: state.car2Left + '%' }" />
    <div class="time-bar-holder">
      <div class="time-bar" :style="{ width: (state.timeLeft / 180) * 100 + 'vw' }"></div>
    </div>
    <div class="type-box">
      <span v-for="(letter, index) in passage" :class="generateClasses(index)">
        {{ letter }}
      </span>
    </div>
  </div>
</template>

<style scoped>
.car-background {
  background-image: url('https://media.istockphoto.com/id/164070810/vector/racing-horizontal-backdrop.jpg?s=612x612&w=0&k=20&c=tlY6ctnqhIyFgKFccIGNyhHm0w9OErF59iCMm9vqfDo=');
  background-image: repeat;
  height: 430px;
  width: 200vw;
  padding: 30px;
  margin-bottom: 50px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  animation: slide 120s linear infinite;
}

@keyframes slide {
  0% {
    transform: translate(0);
  }
  50% {
    transform: translate(-100vw);
  }
  100% {
    transform: translate(0);
  }
}
.car {
  height: 100px;
  position: fixed;
  transition: 1s;
}
.car1 {
  top: 90px;
}
.car2 {
  top: 200px;
}
.time-bar-holder {
  display: flex;
  align-items: center;
  justify-content: center;
}
.time-bar {
  width: 100vw;
  margin: 5px 0;
  background-color: red;
  height: 10px;
  transition: 0.3s;
  border-radius: 5px;
}
.type-box {
  border: solid 3px orange;
  padding: 10px;
  margin: 20px;
  border-radius: 5px;
}

span {
  font-size: 30px;
  color: #4e4e4e;
}

.selectedLetter {
  background-color: rgb(0, 0, 0);
  color: white;
}
.correctLetter {
  color: green;
}
.incorrectLetter {
  color: red;
}
</style>
