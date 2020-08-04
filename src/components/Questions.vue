<template>
  <div class="outer">
    <div class="modal">
      <img src="../assets/logo.png" alt="grid gen" class="logo" />
      <p class="question">{{getCurrentQuestion.text}}</p>
      <button>{{getCurrentQuestion.button}}</button>
      <progress v-if="showProgressBar" progress max="100" :value="progress"></progress>
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
      questions: [
        {
          id: 1,
          text: "Welcome, let me help you create a grid!",
          button: "Let's start!",
        },
        {
          id: 2,
          text: "How many rows would you like to have?",
        },
        {
          id: 3,
          text: "And you guessed it, how many columns?",
        },
        {
          id: 4,
          text: "All right. How many seperate areas does your design have?",
        },
      ],
      answers: {
        rows: Number,
        columns: Number,
        areas: Number,
        areaNames: Array,
      },
    };
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
  min-width: 400px;
  text-align: center;
}
.logo {
  margin: 10px auto;
}
.question {
  margin: 20px 0;
  font-size: 15px;
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
  margin: 0 auto;
  background-color: #448ed2;
  border: 0;
  padding: 10px 30px;
  color: #fff;
  font-weight: 600;
  border-radius: 5px;
  cursor: pointer;
}
</style>
