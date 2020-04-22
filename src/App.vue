<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <QuestionBox
      :currentQuestion="questions[index]"
      :next="next"
      v-if="questions.length"
      :increment="increment"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  mounted() {
    fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
      method: "get",
    })
      .then((res) => {
        return res.json();
      })
      .then((resJson) => {
        this.questions = resJson.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
