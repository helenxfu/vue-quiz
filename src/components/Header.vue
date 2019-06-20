<template>
  <div>
    <h1>Quiz App</h1>
    <p>counter: {{numCorrect}}/{{numTotal}}</p>
  </div>
</template>

<script>
export default {
  props: ["numCorrect", "numTotal"]
};
</script>

<style scoped>
</style>