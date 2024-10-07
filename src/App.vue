<script setup lang="ts">
import { ref } from 'vue'
import ImageWordMatch from './components/ImageWordMatch.vue'
import PhraseCompletion from './components/PhraseCompletion.vue'
import GameCreator from './components/GameCreator.vue'

const currentGame = ref('imageWord')

const switchGame = (game: string) => {
  currentGame.value = game
}
</script>

<template>
  <div class="container">
    <h1>Juegos para Aprender Español</h1>
    <div class="game-selector">
      <button @click="switchGame('imageWord')">Relacionar Imágenes y Palabras</button>
      <button @click="switchGame('phraseCompletion')">Completar Frases</button>
      <button @click="switchGame('gameCreator')">Crear Juego</button>
    </div>
    <component :is="currentGame === 'imageWord' ? ImageWordMatch : 
                    currentGame === 'phraseCompletion' ? PhraseCompletion : GameCreator" />
  </div>
</template>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

.game-selector {
  margin-bottom: 2rem;
}

button {
  margin: 0 0.5rem;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>