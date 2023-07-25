<template>
  <ScoreBoard :win-count="this.winCount" lose-count="this.loseCount"/>
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
import ScoreBoard from '@/components/ScoreBoard.vue'

export default {
  name: 'App',
  components: { ScoreBoard },
  data () {
    return {
      question: '',
      submitted: false,
      answers: [],
      correctAnswer: null,
      chosenAnswer: null,
      winCount: 0,
      loseCount: 0
    }
  },
  methods: {

    shuffleAnswers (answers) {
      for (let i = answers.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1))
        const temp = answers[i]
        answers[i] = answers[j]
        answers[j] = temp
      }
      return answers
    },

    getQuestion () {
      this.question = ''
      this.submitted = false
      this.answers = []
      this.correctAnswer = null
      this.chosenAnswer = null
      this.axios.get('https://opentdb.com/api.php?amount=1&category=17&difficulty=medium').then((res) => {
        const questionData = res.data.results[0]
        this.question = questionData.question
        this.correctAnswer = questionData.correct_answer
        this.answers = this.shuffleAnswers(questionData.incorrect_answers.concat(questionData.correct_answer))
      })
    },

    submitAnswer () {
      if (this.chosenAnswer) {
        this.submitted = true
        console.log(this.chosenAnswer, this.correctAnswer)
        if (String(this.chosenAnswer) === String(this.correctAnswer)) {
          this.winCount++
        } else {
          this.loseCount++
        }
      } else {
        alert('please choose an answer')
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
