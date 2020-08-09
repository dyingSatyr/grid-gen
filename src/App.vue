<template>
  <div id="app">
    <Questions v-if="appState === 1" v-on:quizFinished="initGridSetter($event)" />
    <GridSetter
      v-if="appState === 2"
      :gridConfig="gridConfig"
      v-on:gridSetterFinished="initCodeGenerator($event)"
    />
    <CodeGenerator v-if="appState === 3" :finalGrid="finalGrid" />
  </div>
</template>

<script>
import Questions from "./components/Questions.vue";
import GridSetter from "./components/GridSetter.vue";
import CodeGenerator from "./components/CodeGenerator.vue";

export default {
  name: "App",
  data: function () {
    return {
      appState: 1,
      gridConfig: {},
      finalGrid: [],
    };
  },
  components: {
    Questions,
    GridSetter,
    CodeGenerator,
  },
  methods: {
    initGridSetter: function (data) {
      this.appState++;
      this.gridConfig = data;
    },
    initCodeGenerator: function (data) {
      this.appState++;
      this.finalGrid = data;
    },
  },
};
</script>

<style>
body {
  background-color: #f0f0f0;
}
</style>
