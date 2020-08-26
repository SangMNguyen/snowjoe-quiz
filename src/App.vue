<template>
  <div id="app" :class="{ graded: isGraded }">
      <div id="banner">
        <h1>Snow Joe Quiz Application</h1>
      </div>
      <div id="content">
        <h1>Quiz 1 - Life Questions</h1>
        <div v-show="isGraded" id="scoreSection">
          <h2>Result</h2>
          You got {{ score }} / {{ currentQuestions.length }} questions correct
          <div id="percent">{{ Math.round(score/currentQuestions.length*100) }}%</div>
        </div>
        <Question
          v-for="(item,index) in currentQuestions"
          :questionData="item"
          :key="index"
          @change="updateAnswer"
        />
        <button v-on:click="gradeQuiz">Submit</button>
        <div v-show="isMissing" id="missingSection">
          Please answer all questions before submitting. Unanswered question are displayed in yellow.
        </div>
      </div>
  </div>
</template>

<script>
import Question from './components/Question.vue';
const quizData = require("./questions.json");

export default {
  name: 'App',
  data: function() {
    return {
      score: 0,
      isGraded: false,
      isMissing: false,
      currentQuestions: quizData.questions.map((item,index) => {
        return {
          question: item.question,
          options: item.options,
          qNum: `${index + 1}`,
          userAnswer: -1,
          actualAnswer: item.answer,
          needsAnswer: false,
          isCorrect: false,
        };
      })
    }
  },
  methods: {
    updateAnswer(value) {
      this.userAnswer = value;
    },
    gradeQuiz: function() {
      let grade = 0;
      let missingAnswer = false;

      this.currentQuestions.forEach(item => {
        
        item.needsAnswer = (item.userAnswer === -1);
        if(item.needsAnswer) {
          missingAnswer = true;
        }
        
        if(item.userAnswer === item.actualAnswer) {
          item.isCorrect = true;
          grade++;
        }
      });

      this.score = grade;
      this.isGraded = !missingAnswer;
      this.isMissing = missingAnswer;
      if(missingAnswer) {
        setTimeout(function() {document.getElementById("content").scrollTo(0, document.getElementById("content").scrollHeight)}, 1);
      } else {
        document.getElementById("content").scrollTo(0, 0);
      }
    }
  },
  components: {
    Question
  }
}
</script>

<style>

/* App Wide Styles */
html {
  overflow: hidden;
}
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 3.75rem 0 0 0;
  padding: 0;
}

#banner {
  position: fixed;
  top: 0;
  width: 100%;
  padding: 1rem 0;
  text-align: left;
  background-color: darkred;
  color: white;
  margin-bottom: 1rem;
  z-index: 1000;
}

#banner h1 {
  margin: 0 0 0 1rem;
  font-size: 1.5rem;
}

#content {
  height: calc(100vh - 4rem);
  width: 100%;
  overflow-y: auto;
  scroll-behavior: smooth;
}

#scoreSection {
  color: #555555;
}

#scoreSection h2 {
  text-decoration: underline;
  padding: 0;
  margin: 0 0 1rem 0;
}

#percent {
  margin: 1rem 0;
  font-weight: 800;
  color: red;
}

button {
  margin: 0 auto 2rem auto;
  background-color: #22B667;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 0.75rem;
  width: 5rem;
  transition: 0.3s ease;
}

button:hover {
  background-color: #2CD67B;
  transition: 0.3s ease;
}

#missingSection {
  margin-top: -1rem;
  margin-bottom: 3rem;
  color: red;
}
</style>
