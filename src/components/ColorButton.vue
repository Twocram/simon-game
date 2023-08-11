<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  index: Number,
  color: String,
  activeButton: Number,
});

const emits = defineEmits(['buttonClick']);

function buttonClick(index) {
  emits('buttonClick', index);
}
</script>

<template>
  <div
    :class="[
      'game-button',
      props.color,
      { active: props.activeButton === props.index },
    ]"
    @click="buttonClick(props.index)"
  ></div>
</template>

<style scoped>
.game-button {
  width: 100px;
  height: 100px;
  margin: 5px;
  cursor: pointer;
  opacity: 0.4;
  transition: opacity 0.3s ease-in-out;
}

.red {
  background-color: red;
}

.green {
  background-color: green;
}

.blue {
  background-color: blue;
}

.yellow {
  background-color: yellow;
}

.active {
  opacity: 1;
}
</style>
