<script setup>
import PokemonCard from './PokemonCard.vue';

defineProps(['pokemons']);
</script>

<template>
  <div class="pokemon-container">
    <PokemonCard v-for="pokemon in pokemons" :key="pokemon.id" :pokemon="pokemon" />
  </div>
</template>

<style scoped>
.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}
</style>