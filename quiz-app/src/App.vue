<script setup>
import { onMounted } from 'vue'
import { useQuizStore } from './stores/quizStore'

const quiz = useQuizStore()

onMounted(() => {
  quiz.loadWrongFromStorage()
  quiz.loadSets()
})

const onSetChange = (event) => {
  const id = event.target.value
  console.log(event.target.value);
  console.log(quiz.currentSetId);
  console.log(quiz.questions.length);
  if (id) quiz.selectSet(id)
}
</script>

<template>
  <main class="app">
    <h1>Quiz na učení otázek</h1>

    <section>
      <label>Vyber sadu:</label>
      <select @change="onSetChange">
        <option value="">-- vyber --</option>
        <option v-for="set in quiz.sets" :key="set.id" :value="set.id">
          {{ set.name }}
        </option>
      </select>
    </section>

    <section v-if="quiz.currentSetId && quiz.questions.length ">
      <div class="modes">
        <button :class="{ active: quiz.mode === 'all' }" @click="quiz.setMode('all')">Vše</button>
        <button :class="{ active: quiz.mode === 'wrong' }" @click="quiz.setMode('wrong')">Chybné</button>
      </div>

      <div v-if="quiz.currentQuestion">
        <p>{{ quiz.currentQuestion.question }}</p>

        <ul>
          <li v-for="(a, i) in quiz.currentQuestion.answers" :key="i">
            <button @click="quiz.answerQuestion(i)">{{ a }}</button>
          </li>
        </ul>

        <p v-if="quiz.lastAnswerCorrect === true">Správně! ✅</p>
        <p v-else-if="quiz.lastAnswerCorrect === false">Špatně ❌</p>

        <button @click="quiz.prevQuestion">Předchozí</button>
        <button @click="quiz.nextQuestion">Další</button>
      </div>

      <p v-else>V tomto režimu nejsou žádné otázky.</p>
    </section>
  </main>
</template>
