<script setup>
import { ref, onMounted, computed } from 'vue';
import PokemonList from './PokemonList.vue';
import PokemonFilter from './PokemonFilter.vue';

const pokemons = ref([]);
const selectedType = ref(null);
const loading = ref(true);

// Récupérer les Pokémon
const fetchPokemons = async () => {
  loading.value = true; // Début du chargement
  selectedType.value = null; // Réinitialisation du filtre

  try {
    const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151');
    const data = await response.json();

    // Charger les détails de chaque Pokémon
    const detailedPokemons = await Promise.all(
      data.results.map(async (pokemon) => {
        const res = await fetch(pokemon.url);
        return await res.json();
      })
    );

    pokemons.value = detailedPokemons;
  } catch (error) {
    console.error('Erreur lors de la récupération des Pokémon', error);
  } finally {
    loading.value = false; // Fin du chargement
  }
};

// Filtrer les Pokémon par type
const filteredPokemons = computed(() => {
  if (!selectedType.value) return pokemons.value;
  return pokemons.value.filter(pokemon =>
    pokemon.types.some(t => t.type.name === selectedType.value)
  );
});

const filterPokemonsByType = (type) => {
  selectedType.value = type;
};

onMounted(fetchPokemons);
</script>

<template>
  <div>
    <h1>Pokedex</h1>
    <button @click="fetchPokemons">Recharger les Pokémon</button>

    <PokemonFilter @filter="filterPokemonsByType" />

    <div v-if="loading">Chargement...</div>

    <PokemonList :pokemons="filteredPokemons" v-if="!loading" />
  </div>
</template>

<style scoped>
h1 {
  text-align: center;
}

button {
  margin-bottom: 10px;
  padding: 8px 16px;
  font-size: 16px;
  cursor: pointer;
}
</style>