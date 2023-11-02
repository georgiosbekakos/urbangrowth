<template>
  <div class="gh-inputs-container">
    <h2 class="submission-title">{{ title }}</h2>
    <p style="font-family: 'Roboto Mono', monospace;">The Impact of New Buildings  on <br> 5 | 10 | 15 Minutes City Ecosystems.<br> <br> <span class="info-tooltip">
    <sup>[ User Guide ]</sup>
    <span class="tooltip-text">
      1.Land Plot:"Choose a numbered plot for your building."
      <br><br>
      2.Population:"Select the ratio of the new residential/office population."
      <br><br>
      3.Travel mode:"0-Pedestrian, 1-Car, 2-Bike, 3-Bus."
      <br><br>
      4.Range:"Choose a city travel range in minutes: 5, 10 or 15."
      <br><br>
      5.Time of day:"Select the time of day to assess travel patterns."
      <br><br> -New Building Shown in Black-</span>
  </span></p>

    <GeorgiosSlider
      v-bind:title="firstSliderName"
      v-bind:min="1"
      v-bind:max="500"
      v-bind:step="1"
      v-bind:val="firstSlider"
      v-on:updateValue="updateValue"
    />

    <GeorgiosSlider
      v-bind:title="secondSliderName"
      v-bind:min="1"
      v-bind:max="100"
      v-bind:step="1"
      v-bind:val="secondSlider"
      v-on:updateValue="updateValue"
    />

    <GeorgiosSlider
      v-bind:title="sixthSliderName"
      v-bind:min="1"
      v-bind:max="100"
      v-bind:step="1"
      v-bind:val="sixthSlider"
      v-on:updateValue="updateValue"
    />

    <GeorgiosSlider
      v-bind:title="fifthSliderName"
      v-bind:min="0"
      v-bind:max="3"
      v-bind:step="1"
      v-bind:val="fifthSlider"
      v-on:updateValue="updateValue"
    />

    <GeorgiosSlider
      v-bind:title="thirdSliderName"
      v-bind:min="5"
      v-bind:max="15"
      v-bind:step="5"
      v-bind:val="thirdSlider"
      v-on:updateValue="updateValue"
    />

    <GeorgiosSlider
      v-bind:title="fourthSliderName"
      v-bind:min="1"
      v-bind:max="24"
      v-bind:step="1"
      v-bind:val="fourthSlider"
      v-on:updateValue="updateValue"
    />

  <span class="info-tooltip">
  <sup>[Reachable Amenities]</sup>
  <span class="tooltip-text wide-tooltip">
    <span v-for="(item, index) in metadata.slice(3)" :key="item.name">
      {{ item.name }}: {{ item.value }}
      <br>
    </span>
  </span>
</span>

    <div v-if="metadata.length > 0">
  <h2 class="submission-title">Average Trip Data:</h2>
  <ol>
    <li v-for="(item, index) in metadata.slice(0, 3)" :key="item.name">
      {{ item.name }}: {{ item.value }}
    </li>
  </ol>
</div>


  </div>


  <div class="geometry-view">
    <GeometryView
      v-bind:data="computeData"
      v-bind:path="path"
      v-bind:visibility="runToggle"
      v-on:updateMetadata="receiveMetedata"
    />
  </div>
</template>

<script setup>

import { ref, computed } from "vue";

// Import components
//import SliderInput from "commoncomponents/SliderInput.vue";
import GeorgiosSlider from "./components/GeorgiosSlider.vue";
import GeometryView from "./components/GeometryView.vue";
import def from './assets/urban_06.gh'


const path = def



// Define the tittle of your project
const title = ref("Unraveling Urban Growth");

// Define Grasshopper input names
const firstSliderName = ref("Land Plot Number"); //must match the Input name in your GH definition!
const firstSlider = ref(Math.floor(Math.random() * 24) + 1); //default slider value

const secondSliderName = ref("Residential Increment"); //must match the Input name in your GH definition!
const secondSlider = ref(Math.floor(Math.random() * 24) + 1); //default slider value

const sixthSliderName = ref("Office Increment"); //must match the Input name in your GH definition!
const sixthSlider = ref(0); //default slider value

const fifthSliderName = ref("Travel Mode"); //must match the Input name in your GH definition!
const fifthSlider = ref(1); //default slider value

const thirdSliderName = ref("Walk Radius"); //must match the Input name in your GH definition!
const thirdSlider = ref(5); //default slider value

const fourthSliderName = ref("Time of Day"); //must match the Input name in your GH definition!
const fourthSlider = ref(Math.floor(Math.random() * 24) + 1); //default slider value

const toggleName = ref("Land Plot Numbering");
var runToggle = ref(false);

function updateLocal(newValue) {
  runToggle.value = newValue;
}

//const dropdownName = ref("Color");
//const selectedColorIndex = ref(0);

//const colorOptions = [
 // { label: "Red", value: 0 },
 // { label: "Green", value: 1 },
//  { label: "Blue", value: 2 },
//];



///.............................................

let data = ref({});
let metadata = ref([]);

function updateValue(newValue, parameterName) {
  if (parameterName === firstSliderName.value) {
    firstSlider.value = newValue;
  } else if (parameterName === secondSliderName.value) {
    secondSlider.value = newValue;
  } else if (parameterName === sixthSliderName.value) {
    sixthSlider.value = newValue;
  } else if (parameterName === fifthSliderName.value) {
    fifthSlider.value = newValue;
  } else if (parameterName === thirdSliderName.value) {
    thirdSlider.value = newValue;
  } else if (parameterName === fourthSliderName.value) {
    fourthSlider.value = newValue;
  }
}




//function updateDropdown(newValue) {
//  console.log(`updating dropdown value to: ${newValue}`);
//  selectedColorIndex.value = newValue;
//}

function receiveMetedata(newValue) {
  console.log(newValue);
  metadata.value = newValue;
}

// a computed ref. Vue will keep track of this and update it
const computeData = computed(() => {
 data = {
    [firstSliderName.value]: Number(firstSlider.value),
    [secondSliderName.value]: Number(secondSlider.value),
    [sixthSliderName.value]: Number(sixthSlider.value),
    [fifthSliderName.value]: Number(fifthSlider.value),
    [thirdSliderName.value]: Number(thirdSlider.value),
    [fourthSliderName.value]: Number(fourthSlider.value),
  };
  return data;
});

</script>

<style scoped>
.gh-inputs-container {
  display: flex;
  flex-direction: column;
  height: 750px;
  align-items: center;
  padding: 15px;
  margin: 16px;
  border: 2px solid #000;
  border-radius: 100px;
  text-align: center;
}

.geometry-view {
  display: flex;
  margin: 12px;
  border: none;
  min-width: 200px;
  position: inherit;
  border-radius: 100px;
}

.submission-title {
  font-family: 'Roboto Mono', monospace;
  text-align: center;
  padding: 1px;
}

ol li {
  padding: 1px;
  margin-left: 30px;
  font-family: "Roboto Mono", monospace;
}

ol {
  background: #ffb8ff;
  border-radius: 10px;
  padding: 5px 1px;
  margin-left: 1px;
  width: 330px;
  list-style-type: none;
  border-radius: 100px;
}


.info-tooltip {
  position: relative;
  display: inline-block;
  cursor: pointer;
}

.info-tooltip sup {
  color: #555;
  font-size: 0.8em;
}

.tooltip-text {
  visibility: hidden;
  width: 680px;
  background-color: #555;
  color: #fff;
  text-align: center;
  padding: 5px 0;
  border-radius: 6px;
  
  position: absolute;
  z-index: 1;
  top: 100%;
  left: 50%;
  margin-left: -60px;
  opacity: 0;
  transition: opacity 0.3s;
  background-color: #535353;
  color: #ffffff;
}

.info-tooltip:hover .tooltip-text {
  visibility: visible;
  opacity: 1;
}

.tooltip-text.wide-tooltip {
  width: 200px;
}


</style>