<template>
  <div>{{generatedHTML}}</div>
</template>
<script>
export default {
  name: "CodeGenerator",
  props: {
    finalGrid: Object,
  },
  data: function () {
    return {
      generatedHTML: "",
    };
  },
  created: function () {
    console.dir(this.finalGrid);
    let generateHTML = () => {
      let header = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grid Gen</title>
</head>
<body>
    <div class="container">
`;
      let footer = `
    </div>
</body>
</html>
`;
      let content = "";
      this.finalGrid.sections.forEach((section, i) => {
        //Skip newline if last element
        if (i === this.finalGrid.sections.length - 1) {
          content += `\t<div class="${section.name}">Lorem Ipsum</div>`;
        } else {
          //Add element with newline at the end
          content += `\t<div class="${section.name}">Lorem Ipsum</div>
`;
        }
      });
      // console.log(header, content, footer);
      this.generatedHTML = header + content + footer;
    };
    generateHTML();
  },
};
</script>

<style scoped>
</style>