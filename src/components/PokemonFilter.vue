<script setup>
import { ref, defineEmits } from 'vue';

const types = ref([
  'normal', 'fire', 'water', 'grass', 'electric',
  'ice', 'fighting', 'poison', 'ground', 'flying',
  'psychic', 'bug', 'rock', 'ghost', 'dark',
  'dragon', 'steel', 'fairy'
]);

const emit = defineEmits(['filter']);

const selectType = (type) => {
  emit('filter', type);
};
</script>

<template>
  <div class="filter-container">
    <button v-for="type in types" :key="type" @click="selectType(type)">
      {{ type }}
    </button>
  </div>
</template>

<style scoped>
.filter-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 5px;
  margin-bottom: 20px;
}
button {
  padding: 8px;
  border: none;
  background-color: lightblue;
  cursor: pointer;
  border-radius: 5px;
}
button:hover {
  background-color: blue;
  color: white;
}
</style>
