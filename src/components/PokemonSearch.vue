<script setup>
import { ref } from 'vue';

const searchQuery = ref('');
const sortOrder = ref('none');

const emit = defineEmits(['search']);

const onSearch = () => {
  emit('search', { query: searchQuery.value, sort: sortOrder.value });
};
</script>

<template>
  <div class="search-container">
    <input
      v-model="searchQuery"
      @input="onSearch"
      type="text"
      placeholder="Rechercher un Pokémon..."
    />

    <select v-model="sortOrder" @change="onSearch">
      <option value="none">Sans tri</option>
      <option value="asc">A-Z</option>
      <option value="desc">Z-A</option>
    </select>
  </div>
</template>

<style scoped>
.search-container {
  display: flex;
  gap: 10px;
  align-items: center;
}

input, select {
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 16px;
}
</style>