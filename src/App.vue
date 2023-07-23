<template>
  <h1 v-html="this.question"></h1>
  <div class="input-container">
    <template v-for="answer in answers" :key="answer">
      <div class="radio-input">
        <input
          type="radio"
          id="answer"
          :value="answer"
          v-model="chosenAnswer"
          :disabled="this.submitted"
        >
        <label for="answer">{{ answer }}</label>
      </div>
    </template>
  </div>
  <button
    class="submit-btn"
    @click="this.submitAnswer()"
    v-if="!this.submitted"
  >

    Submit
  </button>
  <button
    class="next-btn"
    @click="this.getQuestion()"
    v-if="this.submitted"
  >
    Next Question
  </button>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      question: '',
      submitted: false,
      answers: [],
      correctAnswer: '',
      chosenAnswer: '',
      winCount: 0,
      loseCount: 0
    }
  },
  methods: {
    getQuestion () {
      this.axios.get('https://opentdb.com/api.php?amount=1&category=17&difficulty=medium').then((res) => {
        const questionData = res.data.results[0]
        this.question = questionData.question
        this.correctAnswer = questionData.correct_answer
        this.answers = questionData.incorrect_answers.concat(questionData.correct_answer)
      })
      this.submitted = false
    },

    submitAnswer () {
      this.submitted = true
      console.log(this.chosenAnswer, this.correctAnswer)
      if (String(this.chosenAnswer) === String(this.correctAnswer)) {
        this.winCount++
      } else {
        this.loseCount++
      }
    }
  },
  mounted () {
    this.getQuestion()
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

  .input-container {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    input {

    }
  }
}
</style>
