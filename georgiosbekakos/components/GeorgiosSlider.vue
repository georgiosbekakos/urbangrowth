<template>
  <div>
    <form class="definition-input">
        <label class="input-title" for="range-slider">{{ title }}: {{ sliderValue }}</label>
  
        <input type="range" class="modern-range"
        v-bind:min="min" v-bind:max="max" v-bind:step="step" v-bind:val="val" 
        v-model="sliderValue" 
        v-on:mouseup="sendValueUpate"
        />
    </form>
  </div>
  
  
  </template>

<script setup>
import { ref } from "vue";
// Define properties that you will be able to access from parent component. 
// Those properties will be binded from parent to child. 
// Available JavaScript types: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures
const props = defineProps(['title', 'min', 'max', 'step', 'val'])

const titlec = ref(props.title)
// Define events that will be accessible from parent component
const emits = defineEmits(['updateValue'])
var sliderValue = ref(props.val)

function sendValueUpate()
{
  emits('updateValue', sliderValue.value, titlec.value)
}




</script>

<style>
.modern-range {
  -webkit-appearance: none;
  width: 100%;
  height: 10px;
  border-radius: 5px;
  background: linear-gradient(to right, #000 0%, #8a8a8a 50%, #6d6d6d 50%, #8d8d8d 100%);
  outline: none;
  margin: 8px 0px;
  background-image: -webkit-repeating-linear-gradient(left, #8a8a8a, #8a8a8a 1px, transparent 1px, transparent 4px);
}

.modern-range::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #000;
  cursor: pointer;
}

.modern-range::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #000;
  cursor: pointer;
}

.modern-range::-ms-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #000;
  cursor: pointer;
}



</style>
