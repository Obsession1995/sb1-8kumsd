<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Phrase {
  id: number
  text: string
  missingWord: string
}

const phrases = ref<Phrase[]>([
  { id: 1, text: 'El sol sale por la ______.', missingWord: 'mañana' },
  { id: 2, text: 'Los pájaros vuelan en el ______.', missingWord: 'cielo' },
  { id: 3, text: 'Me gusta comer ______ con leche.', missingWord: 'cereal' },
])

const currentPhrase = ref<Phrase | null>(null)
const userInput = ref('')
const message = ref('')
const score = ref(0)

const loadNextPhrase = () => {
  if (phrases.value.length > 0) {
    const randomIndex = Math.floor(Math.random() * phrases.value.length)
    currentPhrase.value = phrases.value[randomIndex]
    phrases.value.splice(randomIndex, 1)
    userInput.value = ''
    message.value = ''
  } else {
    currentPhrase.value = null
    message.value = '¡Felicidades! Has completado todas las frases.'
  }
}

const checkAnswer = () => {
  if (currentPhrase.value && userInput.value.toLowerCase() === currentPhrase.value.missingWord.toLowerCase()) {
    score.value++
    message.value = '¡Correcto! Muy bien.'
    setTimeout(loadNextPhrase, 1500)
  } else {
    message.value = 'Incorrecto. Intenta de nuevo.'
  }
}

onMounted(() => {
  loadNextPhrase()
})
</script>

<template>
  <div class="game">
    <h2>Completa la Frase</h2>
    <p>Puntuación: {{ score }}</p>
    <div v-if="currentPhrase" class="phrase-container">
      <p>{{ currentPhrase.text }}</p>
      <input v-model="userInput" @keyup.enter="checkAnswer" placeholder="Escribe la palabra faltante">
      <button @click="checkAnswer">Comprobar</button>
    </div>
    <p class="message">{{ message }}</p>
  </div>
</template>

<style scoped>
.phrase-container {
  margin-top: 2rem;
}

input {
  margin: 1rem 0;
  padding: 0.5rem;
  font-size: 1rem;
}

button {
  margin-left: 0.5rem;
}

.message {
  margin-top: 1rem;
  font-weight: bold;
}
</style>