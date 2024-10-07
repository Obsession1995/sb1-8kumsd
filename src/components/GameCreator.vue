<script setup lang="ts">
import { ref } from 'vue'

const gameType = ref('imageWord')
const imageWordPairs = ref([{ image: '', word: '' }])
const phrase = ref({ text: '', missingWord: '' })
const message = ref('')

const addImageWordPair = () => {
  imageWordPairs.value.push({ image: '', word: '' })
}

const removeImageWordPair = (index: number) => {
  imageWordPairs.value.splice(index, 1)
}

const createGame = () => {
  if (gameType.value === 'imageWord') {
    if (imageWordPairs.value.some(pair => !pair.image || !pair.word)) {
      message.value = 'Por favor, completa todos los campos de imagen y palabra.'
      return
    }
    // Here you would typically save the game data to a backend or local storage
    message.value = 'Juego de relacionar imágenes y palabras creado con éxito.'
  } else if (gameType.value === 'phraseCompletion') {
    if (!phrase.value.text || !phrase.value.missingWord) {
      message.value = 'Por favor, completa la frase y la palabra faltante.'
      return
    }
    // Here you would typically save the game data to a backend or local storage
    message.value = 'Juego de completar frase creado con éxito.'
  }
}
</script>

<template>
  <div class="game-creator">
    <h2>Crear Nuevo Juego</h2>
    <div>
      <label>
        Tipo de Juego:
        <select v-model="gameType">
          <option value="imageWord">Relacionar Imágenes y Palabras</option>
          <option value="phraseCompletion">Completar Frase</option>
        </select>
      </label>
    </div>

    <div v-if="gameType === 'imageWord'">
      <h3>Pares de Imagen y Palabra</h3>
      <div v-for="(pair, index) in imageWordPairs" :key="index" class="pair">
        <input v-model="pair.image" placeholder="Emoji o URL de imagen">
        <input v-model="pair.word" placeholder="Palabra">
        <button @click="removeImageWordPair(index)" v-if="imageWordPairs.length > 1">Eliminar</button>
      </div>
      <button @click="addImageWordPair">Añadir Par</button>
    </div>

    <div v-if="gameType === 'phraseCompletion'">
      <h3>Frase para Completar</h3>
      <input v-model="phrase.text" placeholder="Frase con espacio en blanco (usa ______ para el espacio)">
      <input v-model="phrase.missingWord" placeholder="Palabra faltante">
    </div>

    <button @click="createGame" class="create-button">Crear Juego</button>
    <p class="message">{{ message }}</p>
  </div>
</template>

<style scoped>
.game-creator {
  max-width: 600px;
  margin: 0 auto;
}

.pair {
  margin-bottom: 1rem;
}

input, select {
  margin: 0.5rem;
  padding: 0.5rem;
  font-size: 1rem;
}

.create-button {
  margin-top: 1rem;
  background-color: #4CAF50;
  color: white;
}

.message {
  margin-top: 1rem;
  font-weight: bold;
}
</style>