<template>
  <div>Hellooooo</div>
</template>
<script>
export default {
  name: "CodeGenerator",
  props: {
    finalGrid: Array,
  },
  created: function () {
    console.dir(this.finalGrid);
  },
};
</script>

<style scoped>
</style>