<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface ImageWord {
  id: number
  image: string
  word: string
}

const imageWords = ref<ImageWord[]>([
  { id: 1, image: '🍎', word: 'manzana' },
  { id: 2, image: '🐶', word: 'perro' },
  { id: 3, image: '🏠', word: 'casa' },
  { id: 4, image: '🌞', word: 'sol' },
])

const shuffledWords = ref<string[]>([])
const selectedImage = ref<number | null>(null)
const selectedWord = ref<string | null>(null)
const score = ref(0)

const shuffleWords = () => {
  shuffledWords.value = imageWords.value.map(iw => iw.word).sort(() => Math.random() - 0.5)
}

const selectImage = (id: number) => {
  selectedImage.value = id
  checkMatch()
}

const selectWord = (word: string) => {
  selectedWord.value = word
  checkMatch()
}

const checkMatch = () => {
  if (selectedImage.value !== null && selectedWord.value !== null) {
    const matchedImageWord = imageWords.value.find(iw => iw.id === selectedImage.value)
    if (matchedImageWord && matchedImageWord.word === selectedWord.value) {
      score.value++
      imageWords.value = imageWords.value.filter(iw => iw.id !== selectedImage.value)
      shuffledWords.value = shuffledWords.value.filter(w => w !== selectedWord.value)
    }
    selectedImage.value = null
    selectedWord.value = null
  }
}

onMounted(() => {
  shuffleWords()
})
</script>

<template>
  <div class="game">
    <h2>Relaciona las Imágenes con las Palabras</h2>
    <p>Puntuación: {{ score }}</p>
    <div class="game-board">
      <div class="images">
        <button v-for="iw in imageWords" :key="iw.id" @click="selectImage(iw.id)"
                :class="{ selected: selectedImage === iw.id }">
          {{ iw.image }}
        </button>
      </div>
      <div class="words">
        <button v-for="word in shuffledWords" :key="word" @click="selectWord(word)"
                :class="{ selected: selectedWord === word }">
          {{ word }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.game-board {
  display: flex;
  justify-content: space-around;
  margin-top: 2rem;
}

.images, .words {
  display: flex;
  flex-direction: column;
}

button {
  margin: 0.5rem;
  padding: 1rem;
  font-size: 2rem;
  background-color: #f0f0f0;
  border: 2px solid #ccc;
  border-radius: 8px;
  cursor: pointer;
}

button.selected {
  background-color: #4CAF50;
  color: white;
}
</style>