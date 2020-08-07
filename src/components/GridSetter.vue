<template>
  <div class="container">
    <h1>Select a grid section and check which areas should it cover.</h1>
    <div class="grid-sections-picker">
      <ul>
        <li v-for="(section, index) in gridSections" :key="index">
          <button @click="selectSection(section)" :class="section">{{section}}</button>
        </li>
      </ul>
    </div>
    <h1>Grid areas:</h1>
    <div class="grid-picker">
      <div class="grid-picker-row" v-for="(row, index) in gridGenerated" :key="index">
        <div
          class="grid-picker-box"
          :class="element.name"
          v-for="(element, index) in row"
          :key="index"
          @click="setArea(element.name)"
        >{{element.name}}</div>
      </div>
    </div>
    <button @click="generateCode" :disabled="!readyToGenerateCode">Generate Code</button>
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
      readyToGenerateCode: false,
      selectedSection: null,
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
  methods: {
    generateCode: function () {
      console.log("generate");
    },
    setArea: function (section) {
      if (!this.selectedSection) {
        console.log("No section selected.");
        return;
      }
      console.log(`${section} clicked`);
    },
    selectSection: function (section) {
      let alreadySelected = document.querySelector(".selected");
      if (alreadySelected) {
        alreadySelected.classList.remove("selected");
      }
      let element = document.querySelector("." + section);
      element.classList.add("selected");
      this.selectedSection = section;
    },
  },
};
</script>
<style scoped>
.container {
  margin: 20px;
  padding: 20px;
  font-family: "Montserrat", sans-serif;
  background: #fff;
  box-shadow: 0 1px 3px #d1d1d1;
  border-radius: 3px;
}

.container h1 {
  font-weight: 600;
  font-size: 20px;
  color: #373737;
}

.grid-picker-row {
  display: flex;
  align-content: space-around;
  padding: 5px;
  background: #f0f0f0;
  border-radius: 5px;
  margin-top: 10px;
}

.grid-picker-box {
  background: #373737;

  flex-grow: 1;
  text-align: center;
  padding: 20px;
  margin: 5px;
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

.grid-sections-picker {
  background: #f0f0f0;
  margin: 20px 0;
  padding: 20px;
  border-radius: 5px;
}

.grid-sections-picker ul li {
  display: inline-block;
  margin: 0 5px;
  font-size: 13px;
}

.grid-sections-picker ul li button.selected {
  background-color: #fe638f;
}

button {
  margin: 10px 0;
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

button:disabled {
  background: #d1d1d1;
  cursor: unset;
}

button:disabled:hover {
  background: #d1d1d1;
}

button:hover {
  background-color: #14b3d2;
}
</style>
