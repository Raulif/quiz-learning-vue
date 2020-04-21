<template>
  <div class="outer-wrapper">
    <div class="question-box-container">
      <p class="question-text">{{ currentQuestion.question }}</p>
      <ul class="answer-list">
        <li class="answer-option">
          <button
            v-for="(answer, index) in shuffledAnswers"
            :key="index"
            @click="selectAnswer(index)"
            :class="answerClass(index)"
          >
            {{ answer }}
          </button>
        </li>
      </ul>
      <div>
        <button
          @click="submitAnswer"
          class="button submit-button"
          :disabled="selectedIndex === null || answered"
        >
          Submit
        </button>
        <button class="button next-button" @click="next">Next</button>
      </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
      },
    },
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.answered = true;
    },
    shuffleAnswers() {
      const { incorrect_answers, correct_answer } = this.currentQuestion;
      let answers = [...incorrect_answers, correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(correct_answer);
    },
    answerClass(index) {
      if (!this.answered && this.selectedIndex === index) {
        return "selected";
      }
      if (this.answered && this.correctIndex === index) {
        return "correct";
      }
      if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        return "incorrect";
      }
      return "";
    },
  },
};
</script>

<style scoped>
.outer-wrapper {
  width: 100%;
}

.question-box-container {
  margin: 0 auto;
  width: 50%;
  display: flex;
  flex-direction: column;
  background-color: #eee;
  padding: 1em;
}

.answer-list {
  list-style: none;
  padding-left: 0;
  display: flex;
  flex-direction: column;
  border: 1px solid lightgrey;
  border-bottom: 0;
}

.question-text {
  min-height: 75px;
}

.answer-option > button {
  width: 100%;
  padding: 1em;
  box-shadow: none;
  border: none;
  border-bottom: 1px solid lightgrey;
  cursor: pointer;
  background-color: white;
}

.answer-option > button:hover {
  background-color: #f1f1f1;
}

button.selected {
  background-color: lightblue;
}

button.selected:hover {
  background-color: lightblue;
}

.answer-option > button.correct {
  background-color: lightgreen;
}

.answer-option > button.incorrect {
  background-color: red;
}

.button {
  padding: 1em;
  border: none;
  border-radius: 3px;
  color: white;
  cursor: pointer;
}

.submit-button {
  background-color: royalblue;
  margin-right: 1em;
}

.next-button {
  background-color: forestgreen;
}

.button:hover {
  opacity: 0.9;
}
</style>
