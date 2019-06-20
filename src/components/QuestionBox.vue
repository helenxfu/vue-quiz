<template>
  <div>
    <h2>{{question.question}}</h2>
    <ul>
      <li
        v-for="(answer, index) in shuffledAnswers"
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="answerClass(index)"
      >{{answer}}</li>
      <!-- :class="[
        !answered && selectedIndex === index ? 'selected' : answered && correctIndex === index ? 'correct' : answered && selectedIndex === index && correctIndex !== index ?'incorrect' : ''
      ]"-->
    </ul>
    <button @click="submitAnswer" :disabled="selectedIndex === null || answered">submit</button>
    <button @click="next">next</button>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "QuestionBox",
  props: {
    question: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers];
      answers.push(this.question.correct_answer);
      return answers;
    }
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
    // question() {
    //   this.selectedIndex = null;
    //   this.shuffleAnswers();
    // }
  },
  methods: {
    selectAnswer(index) {
      console.log(index);
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [
        ...this.question.incorrect_answers,
        this.question.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.question.correct_answer
      );
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    }
  },
  mounted() {
    // this.shuffleAnswers();
  }
};
</script>

<style scoped>
li {
  list-style-type: none;
  padding: 5px;
}
li:hover {
  color: rgb(78, 7, 7);
  cursor: pointer;
}
.selected {
  color: blue;
}
.correct {
  color: green;
}
.incorrect {
  color: red;
}
</style>