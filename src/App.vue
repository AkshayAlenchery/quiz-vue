<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <QuestionBox
      :currentQuestion="questions[index]"
      :next="next"
      v-if="questions.length && numTotal !== 4"
      :increment="increment"
    />
    <ResultBox :numCorrect="numCorrect" :numTotal="numTotal" v-if="numTotal === 4"></ResultBox>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import ResultBox from './components/ResultBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    ResultBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  mounted: async function() {
    const resp = await fetch(
      'https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple'
    )
    const result = await resp.json()
    this.questions = result.results
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
