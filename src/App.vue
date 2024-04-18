<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    question: 'How many books are in the kjva bible?',
    answer: 0,
    options: ['80', '32', '60'],
    selected: null
  },
  {
    question: "What is the name of Israel's first king?",
    answer: 2,
    options: ['David', 'Asa', 'Saul'],
    selected: null
  },
  {
    question: 'Salvation is of the...?',
    answer: 1,
    options: ['Egyptians', 'Jews', 'Edomites'],
    selected: null
  }
])

const quizCompleted = ref(false)
const currentIndex = ref(0)
const score = computed(() =>
  questions.value.reduce(
    (acc, item) => acc + +(item.selected === item.answer),
    0
  )
)

const nextQuestion = () => {
  if (currentIndex.value < questions.value.length - 1) {
    currentIndex.value++
  } else {
    quizCompleted.value = true
  }
}
</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>

    <section
      class="quiz" v-if="!quizCompleted"
      v-for="current in [questions[currentIndex]]"
      :key="current.question"
    >
      <div class="quiz-info">
        <div class="question">{{ current.question }}</div>
        <div class="score">Score {{ score }}/{{ questions.length }}</div>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in current.options"
          :key="index"
          :class="`option ${
            current.selected === index
              ? index === current.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            current.selected !== null && index !== current.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            name="options"
            :value="index"
            v-model.number="current.selected"
            :disabled="current.selected !== null"
          />
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="nextQuestion" :disabled="current.selected === null">
        {{
          currentIndex === questions.length - 1
            ? 'Finish'
            : current.selected === null
              ? 'Select an option'
              : 'Next Question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>You Have Finished The Quiz!</h2>
      <p>Your Score is {{ score }} / {{ questions.length }}</p>
      </section>

  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #271c36;
  color: #fff;
}
.options {
  display: flex;
}
label {
  padding: .5rem 1rem;
}
pre {
  font-family: monospace;
  margin-top: 1rem;
  background-color: #eee2;
  border: #fff3 solid;
  border-width: 1px 0;
  padding: 0.5rem;
}
</style>
