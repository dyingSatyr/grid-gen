<template>
  <div id="wrapper">
    <div class="code-display">
      <h1>index.html:</h1>
      <vue-code-highlight language="html">{{generatedHTML}}</vue-code-highlight>
    </div>
    <div class="code-display">
      <h1>grid.css:</h1>
      <vue-code-highlight language="css">{{generatedCSS}}</vue-code-highlight>
    </div>
  </div>
</template>
<script>
import { component as VueCodeHighlight } from "vue-code-highlight";
import "prism-es6/components/prism-markup-templating";
import "prism-es6/components/prism-markup";
import "prism-es6/components/prism-css";
export default {
  name: "CodeGenerator",
  props: {
    finalGrid: Object,
  },
  data: function () {
    return {
      generatedHTML: "",
      generatedCSS: "",
    };
  },
  components: {
    VueCodeHighlight,
  },
  created: function () {
    //console.dir(this.finalGrid);
    let generateHTML = () => {
      let header = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="grid.css" type="text/css">
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
      //console.log(header, content, footer);
      this.generatedHTML = header + content + footer;
    };

    let generateCSS = () => {
      //console.log(this.finalGrid);

      //Add container styles
      let contentCSS = `
html, body {
  margin: 0;
  padding: 0;
}

.container {
  display:grid;
  min-height:100vh;
  grid-template-columns:`;
      //Add grid template columns for each column
      for (let i = 0; i < this.finalGrid.gridGenerated[0].length; i++) {
        contentCSS += ` 1fr`;
      }
      contentCSS += `;`;

      //Add grid template areas
      contentCSS += `
  grid-template-areas: `;

      this.finalGrid.gridGenerated.forEach((row) => {
        contentCSS += `
        "`;
        row.forEach((obj, index) => {
          if (index == row.length - 1) {
            contentCSS += `${obj.section}`;
          } else {
            contentCSS += `${obj.section} `;
          }
        });
        contentCSS += `"`;
      });
      contentCSS += `;
}
`;

      //Assign area for each section
      this.finalGrid.sections.forEach((section) => {
        contentCSS += `
.${section.name} {
  grid-area: ${section.name};
  background-color: ${section.color};
}
`;
      });

      this.generatedCSS = contentCSS;
    };
    generateCSS();
    generateHTML();
  },
};
</script>

<style scoped>
#wrapper {
  display: flex;
  justify-content: center;
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
  font-family: "Montserrat", sans-serif;
}

#wrapper h1 {
  font-weight: 600;
  font-size: 20px;
}

.code-display {
  margin: 20px;
}

.code-display > div {
  width: 600px;
}
</style>