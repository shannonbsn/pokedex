<script setup>
import { ref, onMounted, computed } from 'vue';
import PokemonList from './PokemonList.vue';
import PokemonFilter from './PokemonFilter.vue';
import PokemonSearch from './PokemonSearch.vue';

const pokemons = ref([]);
const selectedType = ref(null);
const searchQuery = ref('');
const sortOrder = ref('none');
const loading = ref(true);

const fetchPokemons = async () => {
  loading.value = true;
  selectedType.value = null;
  searchQuery.value = '';
  sortOrder.value = 'none';

  try {
    const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151');
    const data = await response.json();

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
    loading.value = false;
  }
};

const filteredPokemons = computed(() => {
  let filtered = [...pokemons.value];

  if (selectedType.value) {
    filtered = filtered.filter(pokemon =>
      pokemon.types.some(t => t.type.name === selectedType.value)
    );
  }

  if (searchQuery.value) {
    filtered = filtered.filter(pokemon =>
      pokemon.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  }

  if (sortOrder.value === 'asc') {
    filtered.sort((a, b) => a.name.localeCompare(b.name));
  } else if (sortOrder.value === 'desc') {
    filtered.sort((a, b) => b.name.localeCompare(a.name));
  }

  return filtered;
});

const updateSearch = ({ query, sort }) => {
  searchQuery.value = query;
  sortOrder.value = sort;
};

const filterPokemonsByType = (type) => {
  if (selectedType.value === type) {
    selectedType.value = null;
  } else {
    selectedType.value = type;
  }
};

onMounted(fetchPokemons);
</script>

<template>
  <div>
    <h1>Pokedex</h1>
    <button @click="fetchPokemons">Recharger les Pokémon</button>

    <PokemonSearch @search="updateSearch" />
    <PokemonFilter @filter="filterPokemonsByType" :selected="selectedType" />

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
