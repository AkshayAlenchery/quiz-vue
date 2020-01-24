<template>
  <div>
    <b-nav>
      <b-nav-item disabled><b>My Quiz App</b></b-nav-item>
      <b-nav-item disabled>Score: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    numCorrect: Number,
    numTotal: Number
  }
}
</script>
