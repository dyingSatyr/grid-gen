<template>
  <div class="container">
    <h1>Select a grid section and check which areas should it cover.</h1>
    <div class="grid-sections-picker">
      <ul>
        <li v-for="(section, index) in gridSections" :key="index">
          <button
            @click="selectSection(section)"
            :class="section.name"
            :style="{'background-color': section.color}"
          >{{section.name}}</button>
        </li>
      </ul>
    </div>
    <h1>Grid areas:</h1>
    <div class="grid-picker">
      <div class="grid-picker-row" v-for="(row, rowIndex) in gridGenerated" :key="rowIndex">
        <div
          class="grid-picker-box"
          :class="element.name"
          v-for="(element, index) in row"
          :key="index"
          @click="setArea(element, rowIndex)"
        >{{element.name}}</div>
      </div>
    </div>
    <button
      class="btn-generate"
      @click="generateCode"
      :disabled="!readyToGenerateCode"
    >Generate Code</button>
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
    //Generate grid data
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
            section: null, //This will be filled in with appropriate section once clicked
          };
        }
      }
    };
    //Generate sections (divs, sections, asides)
    let generateSections = (gridCnf) => {
      for (let i = 0; i < gridCnf.areas; i++) {
        this.gridSections.push({
          name: `section-${i + 1}`,
          color: this.getRandomColor(),
        });
      }
    };
    generateSections(this.gridConfig);
    generateGrid(this.gridConfig);
  },
  methods: {
    //helper func to generate different colors for each section
    getRandomColor: function () {
      let letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    //Finish step 2 and move on to showing code
    generateCode: function () {
      this.$emit("gridSetterFinished", this.gridGenerated);
    },
    setArea: function (area, row) {
      //Do nothing if no section is already selected
      if (!this.selectedSection) {
        console.log("No section selected.");
        return;
      }
      //Set color to match the section
      let element = document.querySelector("." + area.name);
      element.style.backgroundColor = this.selectedSection.color;
      //Assign a section to data in generatedGrid
      let gEle = this.gridGenerated[row].find((gridBox) => {
        return gridBox.name === area.name;
      });
      gEle.section = this.selectedSection.name;
      this.readyToGenerateCodex();
    },
    selectSection: function (section) {
      //Check if something already selected and deselect it
      let alreadySelected = document.querySelector(".selected");
      if (alreadySelected) {
        alreadySelected.classList.remove("selected");
      }
      //Add selected class to element
      let element = document.querySelector("." + section.name);
      element.classList.add("selected");
      //Set the element as selected in data
      this.selectedSection = section;
    },
    //Check if all areas have sections assigned
    readyToGenerateCodex: function () {
      //Check if any sections are null
      for (let i = 0; i < this.gridGenerated.length; i++) {
        let isEmpty = this.gridGenerated[i].some((obj) => obj.section === null);
        if (isEmpty) {
          this.readyToGenerateCode = false;
          return;
        }
      }
      this.readyToGenerateCode = true;
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
}

.grid-sections-picker ul li button {
  font-size: 13px;
  border: 2px solid #f0f0f0;
  margin: 0 10px;
  padding: 10px 20px;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  outline: none;
}

.grid-sections-picker ul li button.selected {
  border-color: #373737;
}

button.btn-generate {
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

button.btn-generate:disabled {
  background: #d1d1d1;
  cursor: unset;
}

button.btn-generate:disabled:hover {
  background: #d1d1d1;
}

button.btn-generate:hover {
  background-color: #14b3d2;
}
</style>
