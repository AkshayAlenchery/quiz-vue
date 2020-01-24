<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectedAns(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        >Submit</b-button
      >
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers()
        this.answered = false
      }
    }
  },
  methods: {
    selectedAns: function(index) {
      this.selectedIndex = index
    },

    shuffleAnswers: function() {
      const answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer: function() {
      let isCorrect = false
      if (this.selectedIndex == this.correctIndex) {
        isCorrect = true
      }
      this.increment(isCorrect)
      this.answered = true
    },
    answerClass: function(index) {
      let answerClass = ''
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = 'wrong'
      }
      return answerClass
    }
  }
}
</script>

<style scoped>
.list-group {
  margin-bottom: 20px;
}

.list-group-item {
  cursor: pointer;
}

.list-group-item:hover {
  background: lightcyan;
}

.selected {
  background: lightblue;
}

.correct {
  background: lightgreen;
}

.wrong {
  background: lightcoral;
}

.btn {
  margin: 0 5px;
}
</style>
