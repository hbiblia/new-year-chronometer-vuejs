<template>
  <canvas id="canvas-confetti"></canvas>
  <div class="main">
    <div class="new_year">{{ language[0] }} {{ new_year }}</div>
    <div class="cronometro">
      <span v-if="day != 0">{{ day }} </span>
      <span v-if="day != 0" class="text-day"> {{ language[1] }} </span>
      <span v-if="hour != 0">{{ hour }}:</span>
      <span v-if="min != 0">{{ min }}:</span>
      {{ second }}
    </div>
    <div class="new_year_full">{{ new_date.toDateString() }}</div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ConfettiGenerator from "confetti-js";

let language_id = navigator.language.split('-')[0]
let language_text = {
  "es":['Año Nuevo', 'días'],
  "en":['New Year','days'],
  "fr":['Nouvelle année','jours'],
}
let language = language_text[language_id]

if (language_text[language_id] == undefined){
  language = language_text['es']
}

let current_date = new Date()
let new_date = new Date(`${(current_date.getFullYear() + 1)}-1-01`)

const current_year = current_date.getFullYear();
const new_year = ref(current_date.getFullYear() + 1)

let confetti, timeObject;

let day = ref(0)
let hour = ref(0)
let min = ref(0)
let second = ref(0)

function op_micro() {
  let op_date = new_date.getTime() - new Date().getTime()
  let time_second = (op_date / 1000)

  second.value = parseInt(time_second % 60) 
  min.value = parseInt(time_second / 60 % 60)
  hour.value = parseInt(time_second / 60 / 60 % 24)
  day.value = parseInt(time_second / (24*3600))

  if (day.value == 0 && hour.value == 0 && min.value == 0 && second.value == 0) {
    confetti.render();
    clearInterval(timeObject)
  }
}

onMounted(() => {
  timeObject = setInterval(() => {
    op_micro()
  }, 1000)

  confetti = new ConfettiGenerator({ target: document.getElementById('canvas-confetti') })
})

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Fredoka+One&family=Roboto:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  overflow: hidden;
}

.new_year {
  font-size: 40px;
  font-family: 'Fredoka One', cursive;
}

.cronometro {
  font-size: 120px;
  font-family: 'Roboto', sans-serif;
}

.new_year_full {
  font-size: 30px;
}

.text-day {
  font-size: 40px;
}

.main {
  margin: auto;
  width: fit-content;
  height: fit-content;

  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  z-index: 1;
}

.canvas-confetti {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  z-index: 0;
}
</style>
