<template>
  <div class="outer">
    <div class="modal">
      <img src="../assets/logo.png" alt="grid gen" class="logo" />
      <progress v-if="showProgressBar" progress max="100" :value="progress"></progress>
      <div v-if="currentQuestion === 1" class="question-container">
        <p class="question">Welcome, let me help you create a grid!</p>
        <button @click="startQuiz">Let's start!</button>
      </div>
      <div v-else-if="currentQuestion === 2" class="question-container">
        <p class="question">How many rows would you like to have?</p>
        <p v-if="errorMessage" class="error">{{errorMessage}}</p>
        <input v-model="userInput" class="answer-box" type="text" placeholder="Number of rows..." />
        <button @click="setRows">OK</button>
      </div>
      <div v-else-if="currentQuestion === 3" class="question-container">
        <p class="question">And you guessed it, how many columns?</p>
        <p v-if="errorMessage" class="error">{{errorMessage}}</p>
        <input
          v-model="userInput"
          class="answer-box"
          type="text"
          placeholder="Number of columns..."
        />
        <button @click="setColumns">OK</button>
        <button @click="back" class="back-btn">Back</button>
      </div>
      <div v-else-if="currentQuestion === 4" class="question-container">
        <p class="question">All right. How many sections does your design have?</p>
        <p v-if="errorMessage" class="error">{{errorMessage}}</p>
        <input
          v-model="userInput"
          class="answer-box"
          type="text"
          placeholder="Number of sections..."
        />
        <button @click="setSections">OK</button>
        <button @click="back" class="back-btn">Back</button>
      </div>
      <div v-else-if="currentQuestion === 5" class="question-container">
        <p class="question">I think we have everything we need for now.</p>
        <button @click="finishQuiz">Finish</button>
        <button @click="back" class="back-btn">Back</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Questions",
  data: function () {
    return {
      currentQuestion: 1,
      progress: 0,
      showProgressBar: false,
      errorMessage: "",
      userInput: "",
      answers: {
        rows: null,
        columns: null,
        areas: null,
      },
    };
  },
  methods: {
    startQuiz: function () {
      this.showProgressBar = true;
      this.currentQuestion++;
    },
    setRows: function () {
      if (!this.userInput || !Number.isInteger(parseInt(this.userInput, 10))) {
        this.errorMessage = "Woah, woah, woah. Only numbers are allowed.";
        return;
      }
      this.errorMessage = "";
      this.answers.rows = this.userInput;
      this.userInput = "";
      this.progress += 25;
      this.currentQuestion++;
    },
    setColumns: function () {
      if (!this.userInput || !Number.isInteger(parseInt(this.userInput, 10))) {
        this.errorMessage = "Woah, woah, woah. Only numbers are allowed.";
        return;
      }
      this.errorMessage = "";
      this.answers.columns = this.userInput;
      this.userInput = "";
      this.progress += 25;
      this.currentQuestion++;
    },
    setSections: function () {
      if (!this.userInput || !Number.isInteger(parseInt(this.userInput, 10))) {
        this.errorMessage = "Woah, woah, woah. Only numbers are allowed.";
        return;
      }
      if (this.userInput > this.answers.rows * this.answers.columns) {
        this.errorMessage = `Get real bro. You can't fit ${this.userInput} sections into ${this.answers.rows} x ${this.answers.columns} grid.`;
        return;
      }
      this.errorMessage = "";
      this.answers.areas = this.userInput;
      this.userInput = "";
      this.progress += 25;
      this.currentQuestion++;
    },
    finishQuiz: function () {
      this.$emit("quizFinished", this.answers);
    },
    back: function () {
      this.errorMessage = "";
      this.userInput = "";
      this.currentQuestion--;
      this.progress -= 25;
    },
  },
};
</script>

<style scoped>
.outer {
  font-family: "Montserrat", sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}
.modal {
  background-color: #fff;
  border: 1px solid #d1d1d1;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 3px 3px #d1d1d1;
  width: 400px;
  text-align: center;
}
.logo {
  margin: 10px auto;
}
.question {
  margin: 10px 0;
  font-size: 15px;
  line-height: 1.7em;
  color: #555;
}

.error {
  background-color: #d65378;
  padding: 10px;
  font-size: 13px;
  color: #fff;
  border-radius: 5px;
}

.answer-box {
  display: block;
  width: 200px;
  margin: 20px auto;
  padding: 5px;
  border-radius: 3px;
  border: 1px solid #d1d1d1;
  font-size: 14px;
  color: #333;
}

progress {
  appearance: none;
  width: 100%;
  height: 6px;
  background-color: #eee;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.25) inset;
  margin: 20px 0 0 0;
}

button {
  margin: 10px;
  background-color: #14c3e6;
  border: 0;
  padding: 10px 30px;
  color: #fff;
  font-weight: 600;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s ease-in-out;
}

button:hover {
  background-color: #14b3d2;
}

button.back-btn {
  background-color: #fe638f;
}

button.reset-btn:hover {
  background-color: #d65378;
}
</style>
