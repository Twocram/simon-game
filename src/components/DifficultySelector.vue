<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  levels: Array,
  selectedLevel: String,
});

const emits = defineEmits(['emitChange']);

function emitChange(value) {
  emits('emitChange', value);
}
</script>

<template>
  <div class="difficulty">
    <label>Select Difficulty:</label>
    <label v-for="item in props.levels" :key="item.id">
      <input
        type="radio"
        @change="emitChange(item.value)"
        :checked="item.value === selectedLevel"
        :value="item.value"
      />
      {{ item.title }}
    </label>
  </div>
</template>

<style scoped>
.difficulty {
  margin-top: 10px;
}

label {
  margin-right: 10px;
}
</style>
