<template>
  <div id="app">
    <Questions msg="Grid Gen" />
  </div>
</template>

<script>
import Questions from "./components/Questions.vue";

export default {
  name: "App",
  components: {
    Questions,
  },
};
</script>

<style>
body {
  background-color: #f0f0f0;
}
</style>
