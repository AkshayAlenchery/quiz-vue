<template>
  <div>
    <b-jumbotron>
      <template v-slot:header>Game Over !!</template>
      <template v-slot:lead>Your score is {{ numCorrect }} / {{ numTotal }} </template>
      <p>
        Reload to play again
      </p>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: ['numCorrect', 'numTotal']
}
</script>
