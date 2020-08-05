<template>
  <div class="container">
    <h1>Select a grid section on the left and check which areas should it cover.</h1>
    <div class="grid-sections-picker">
      <ul>
        <li v-for="(section, index) in gridSections" :key="index">{{section}}</li>
      </ul>
    </div>
    <div class="grid-picker">
      <div class="grid-picker-row" v-for="(row, index) in gridGenerated" :key="index">
        <div
          class="grid-picker-box"
          :class="element.name"
          v-for="(element, index) in row"
          :key="index"
        >{{element.name}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "GridSetter",
  props: {
    gridConfig: Array,
  },
  data: function () {
    return {
      gridGenerated: [],
      gridSections: [],
    };
  },
  created() {
    let generateGrid = (gridCnf) => {
      for (let i = 0; i < gridCnf.rows; i++) {
        for (let j = 0; j < gridCnf.columns; j++) {
          //push elements to rows
          if (!this.gridGenerated[i]) {
            this.gridGenerated[i] = [];
          }
          this.gridGenerated[i][j] = {
            name: `area-${i + 1}-${j + 1}`,
            row: i,
            column: j,
          };
        }
      }
    };

    let generateSections = (gridCnf) => {
      for (let i = 0; i < gridCnf.areas; i++) {
        this.gridSections.push(`section-${i + 1}`);
      }
    };
    generateSections(this.gridConfig);
    generateGrid(this.gridConfig);
  },
};
</script>
<style scoped>
.container {
  margin: 20px auto;
  font-family: "Montserrat", sans-serif;
  background: #fff;
  box-shadow: 0 1px 3px #d1d1d1;
  border-radius: 3px;
}

.container h1 {
  font-weight: 600;
  font-size: 20px;
  padding: 20px;
  color: #373737;
}

.grid-picker-row {
  display: flex;
  align-content: space-between;
}

.grid-picker-box {
  background: #373737;
  align-self: center;
  flex-grow: 1;
  text-align: center;
  padding: 20px;
  margin: 10px;
  color: #b9b9b9;
  font-size: 13px;
  cursor: pointer;
  transition: all 0.1s ease-in;
  border-radius: 5px;
}

.grid-picker-box:hover {
  background: #fe638f;
  color: #fff;
}
</style>
