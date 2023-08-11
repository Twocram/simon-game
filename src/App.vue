<script setup>
import { reactive } from 'vue';
import ColorButton from './components/ColorButton.vue';
import DifficultySelector from './components/DifficultySelector.vue';

const DELAY_BETWEEN_BUTTONS = 600;

const colors = ['red', 'green', 'blue', 'yellow'];

const state = reactive({
  isPlaying: false,
  selectedLevel: 'easy',
  sequence: [],
  userSequence: [],
  activeButton: null,
});

const levels = [
  {
    id: 1,
    title: 'Easy',
    value: 'easy',
    time: 1500,
  },
  {
    id: 2,
    title: 'Medium',
    value: 'medium',
    time: 1000,
  },
  {
    id: 3,
    title: 'Hard',
    value: 'hard',
    time: 400,
  },
];

const buttonSounds = {
  red: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound2.mp3'),
  green: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound1.mp3'),
  blue: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound3.mp3'),
  yellow: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'),
};

const startGame = () => {
  state.isPlaying = true;
  state.sequence = [];
  state.userSequence = [];

  generateButton();
};

const generateButton = async () => {
  const randomButtonIndex = Math.floor(Math.random() * colors.length);
  state.sequence.push(randomButtonIndex);

  await new Promise((resolve) => setTimeout(resolve, DELAY_BETWEEN_BUTTONS));
  state.userSequence = [];
  await playSequence();
};

const playSequence = async () => {
  const timeout = levels.find(
    (item) => item.value === state.selectedLevel
  ).time;

  for (let item of state.sequence) {
    state.activeButton = item;
    buttonSounds[colors[item]].play();
    await new Promise((resolve) => setTimeout(resolve, timeout));
    state.activeButton = null;
    await new Promise((resolve) => setTimeout(resolve, DELAY_BETWEEN_BUTTONS));
  }
};

const handleButtonClick = (index) => {
  if (!state.isPlaying) return;

  state.userSequence.push(index);
  state.activeButton = index;
  buttonSounds[colors[index]].play();
  setTimeout(() => {
    state.activeButton = null;
  }, 300);

  if (arraysEqual(state.userSequence, state.sequence)) {
    if (state.userSequence.length === state.sequence.length) {
      state.userSequence = [];
      generateButton();
    }
  } else if (state.userSequence.length === state.sequence.length) {
    endGame();
  }
};

const arraysEqual = (arr1, arr2) => {
  if (arr1.length !== arr2.length) return false;
  for (let i = 0; i < arr1.length; i++) {
    if (arr1[i] !== arr2[i]) return false;
  }
  return true;
};

const endGame = () => {
  state.isPlaying = false;

  alert('Game Over');
};

const updateSelectedLevel = (newLevel) => {
  state.selectedLevel = newLevel
}
</script>

<template>
  <div class="simon-game">
    <h1>Simon Game</h1>
    <DifficultySelector
      :levels="levels"
      :selected-level="state.selectedLevel"
      @emit-change="updateSelectedLevel"
    />
    <div class="button-container">
      <ColorButton
        v-for="(color, index) in colors"
        :color="color"
        :index="index"
        :active-button="state.activeButton"
        @button-click="handleButtonClick"
      />
    </div>
    <button @click="startGame" :disabled="state.isPlaying">Start Game</button>
  </div>
</template>

<style scoped>
.simon-game {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 100px;
}

.button-row {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.button-container {
  display: grid;
  grid-template-columns: 2fr 2fr;
}
</style>
