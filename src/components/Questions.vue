<template>
  <div class="outer">
    <div class="modal">
      <img src="../assets/logo.png" alt="grid gen" class="logo" />
      <progress v-if="showProgressBar" progress max="100" :value="progress"></progress>
      <p class="question">{{getCurrentQuestion.text}}</p>
      <p v-if="errorMessage" class="error">{{errorMessage}}</p>
      <input
        v-if="getCurrentQuestion.requiresInput"
        v-model="userInput"
        class="answer-box"
        type="text"
        :placeholder="getCurrentQuestion.placeholder"
      />
      <button @click="answer(currentQuestion)">{{getCurrentQuestion.button}}</button>
      <button v-if="currentQuestion > 2" @click="back" class="back-btn">Back</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Questions",
  props: {
    msg: String,
  },
  data: function () {
    return {
      currentQuestion: 1,
      progress: 0,
      showProgressBar: false,
      errorMessage: "",
      userInput: "",
      answers: {
        rows: Number,
        columns: Number,
        areas: Number,
        areaNames: Array,
      },
      questions: [
        {
          id: 1,
          text: "Welcome, let me help you create a grid!",
          button: "Let's start!",
          requiresInput: false,
          placeholder: "",
        },
        {
          id: 2,
          text: "How many rows would you like to have?",
          button: "OK",
          requiresInput: true,
          placeholder: "Number of rows...",
        },
        {
          id: 3,
          text: "And you guessed it, how many columns?",
          button: "OK",
          requiresInput: true,
          placeholder: "Number of columns...",
        },
        {
          id: 4,
          text: "All right. How many sections does your design have?",
          button: "OK",
          requiresInput: true,
          placeholder: "Number of sections...",
        },
        {
          id: 5,
          text: `I think we have everything we need for now.`,
          requiresInput: false,
          button: "Finish",
          placeholder: "",
        },
      ],
    };
  },
  methods: {
    answer: function (qnr) {
      //Validate input
      if (
        (qnr >= 2 && !this.userInput) ||
        (qnr >= 2 && !Number.isInteger(parseInt(this.userInput, 10)))
      ) {
        console.log(this.userInput);
        this.errorMessage = "Woah, woah, woah. Only numbers are allowed.";
        return;
      }
      //Clear error if it was there already
      this.errorMessage = "";

      //Show progress bar at 1st question
      if (qnr >= 1) {
        this.showProgressBar = true;
      }
      //Raise progress at each question after 2nd
      if (qnr > 1) {
        this.progress += 25;
      }
      //store the value user input to data
      this.storeInput(qnr, parseInt(this.userInput, 10));
      console.log(this.answers);
      //Increment current question
      this.currentQuestion++;
      //Empty the input
      this.userInput = "";
    },
    back: function () {
      this.currentQuestion--;
      this.progress -= 25;
    },

    storeInput: function (qnr, input) {
      switch (qnr) {
        case 2:
          this.answers.rows = input;
          break;
        case 3:
          this.answers.columns = input;
          break;
        case 4:
          this.answers.areas = input;
      }
    },
  },
  computed: {
    getCurrentQuestion: function () {
      let q = this.questions.find((obj) => {
        return obj.id === this.currentQuestion;
      });

      return q;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
