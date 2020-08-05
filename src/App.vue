<template>
  <div id="app">
    <Questions v-if="appState === 1" v-on:quizFinished="initGridSetter($event)" />
    <GridSetter v-if="appState === 2" :gridConfig="gridConfig" />
  </div>
</template>

<script>
import Questions from "./components/Questions.vue";
import GridSetter from "./components/GridSetter.vue";

export default {
  name: "App",
  data: function () {
    return {
      appState: 1,
      gridConfig: {},
    };
  },
  components: {
    Questions,
    GridSetter,
  },
  methods: {
    initGridSetter: function (data) {
      this.appState++;
      this.gridConfig = data;
    },
  },
};
</script>

<style>
body {
  background-color: #f0f0f0;
}
</style>
