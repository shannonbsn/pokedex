<script setup>
defineProps(['pokemon']);
</script>

<template>
  <div class="card">
    <img :src="pokemon.sprites.front_default" :alt="pokemon.name">
    <h3>{{ pokemon.name }}</h3>
    <p>
      Type(s) :
      <span v-for="type in pokemon.types" :key="type.type.name">
        {{ type.type.name }}
      </span>
    </p>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
  width: 150px;
  background-color: #f8f8f8;
  border-radius: 10px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
}
img {
  width: 100px;
}
</style>
