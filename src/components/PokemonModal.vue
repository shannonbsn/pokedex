<script setup>
defineProps(['pokemon']);
defineEmits(['close']);
</script>

<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content">
      <h2>#{{ pokemon.id }} - {{ pokemon.name }}</h2>

      <img :src="pokemon.sprites.front_default" :alt="pokemon.name" />

      <p><strong>Type :</strong> {{ pokemon.types.map(t => t.type.name).join(', ') }}</p>
      <p><strong>Taille :</strong> {{ pokemon.height / 10 }} m</p>
      <p><strong>Poids :</strong> {{ pokemon.weight / 10 }} kg</p>

      <div class="section-title">Statistiques :</div>
      <ul class="horizontal-list">
        <li v-for="stat in pokemon.stats" :key="stat.stat.name">
          <strong>{{ stat.stat.name }} :</strong> {{ stat.base_stat }}
        </li>
      </ul>

      <div class="section-title">Capacités :</div>
      <ul class="horizontal-list">
        <li v-for="(move, index) in pokemon.moves.slice(0, 5)" :key="index">
          {{ move.move.name }}
        </li>
      </ul>

      <button @click="$emit('close')">Fermer</button>
    </div>
  </div>
</template>


<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  animation: fadeIn 0.3s ease-in-out;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 12px;
  width: 90%;
  max-width: 500px;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  transform: scale(0.9);
  animation: zoomIn 0.3s ease-in-out forwards;
}

h2 {
  margin-bottom: 10px;
  color: #ff4c4c;
  font-size: 24px;
}

img {
  width: 150px;
  height: 150px;
  object-fit: contain;
  margin: 10px 0;
}

p, .section-title {
  margin: 5px 0;
  font-size: 16px;
  font-weight: bold; /* Optionnel pour mettre en valeur les titres */
}

ul.horizontal-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 15px;
  list-style: none;
  padding: 0;
  margin: 10px 0;
}

li {
  font-size: 14px;
}

button {
  margin-top: 10px;
  padding: 10px 15px;
  font-size: 16px;
  cursor: pointer;
  background: #ff4c4c;
  color: white;
  border: none;
  border-radius: 8px;
  transition: background 0.3s;
}

button:hover {
  background: #d43f3f;
}

/* Effet d’apparition */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes zoomIn {
  from { transform: scale(0.9); }
  to { transform: scale(1); }
}
</style>
