<template>
  <div id="app">
    <h1>Blurdle</h1>
    <p>You have 5 guesses to guess the image. The image gets clearer after every guess.</p>
    <hr>
    <div style="margin-top:1em;">
      <p style="font-size:20px;" v-if="!this.gameOver && !this.correct">Guesses left: {{ this.guessCount }}</p>
      <p class="red" v-if="this.guessEmpty">PLEASE ENTER A GUESS</p>
      <p class="red" v-if="this.incorrect">INCORRECT</p>

      <div v-if="this.gameOver" style="margin-bottom:1em;">
        <p class="red">GAME OVER. THE IMAGE WAS OF A {{ this.imageToShow.answer.toUpperCase() }}</p>
        <button @click="playAgain()">Play again</button>
      </div>

      <div v-if="this.correct" style="margin-bottom:1em;">
        <p class="green" v-if="this.correct">CORRECT, THE IMAGE WAS OF A {{ this.imageToShow.answer.toUpperCase() }}!</p>
        <button @click="playAgain()">Play again</button>
      </div>
      
      <img :src="require(`${this.imageToShow.src}`)"  width="300" height="300" :class="`blur-${guessCount}`">
      <p><input type="text" name="guess" placeholder="Enter guess..." v-model="guess" :disabled="this.disabled"/><br>
      <button @click="validateAnswer()" :disabled="this.disabled">Submit</button></p>
    </div>
  </div>
</template>

<script>
function getRandomInt(min, max) {
  min = Math.ceil(min);
  return Math.floor(Math.random() * (Math.floor(max) - min) + min);
}

import { images } from './images.js';

export default {
  name: 'App',
  data(){
    return {
      class: 'blur-5',
      guessCount: 5,
      guess: '',
      guessEmpty: false,
      incorrect: false,
      correct: false,
      disabled: false,
      gameOver: false,
      imageToShow: images[getRandomInt(0, images.length)],
    }
  },
  methods: {
    validateAnswer() {
      this.guessEmpty = false;
      this.incorrect = false;

      if (this.guess === '') {
        this.guessEmpty = true;
        return;
      }

      if (this.guess.toLowerCase() === this.imageToShow.answer.toLowerCase()) {
        this.correct = true;
        this.disabled = true;
        this.guessCount = 0;
        return;
      } else {
        this.incorrect = true;
        this.guessCount--;
        this.guess = '';
      }

      if (this.guessCount == 0) {
        this.gameOver = true;
        this.disabled = true;
        return;
      }
    },
    playAgain() {
      this.guess = '';
      this.disabled = false;
      this.correct = false;
      this.incorrect = false;
      this.guessCount = 5;
      this.gameOver = false;

      this.imageToShow = images[getRandomInt(0, images.length)];
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 1em;
}

.blur-5{
  filter: blur(5rem);
}
.blur-4{
  filter: blur(4rem);
}
.blur-3{
  filter: blur(3rem);
}
.blur-2{
  filter: blur(2rem);
}
.blur-1{
  filter: blur(1rem);
}
.blur-0{
  filter: blur(0rem);
}
.red {
  font-size:20px;
  color:red;
}
.green {
  font-size:20px;
  color:green;
}
button {
  border:1px solid grey;
  background:white;
  color:black;
  margin-top:0.2em;
  padding:1em;
}
button:hover {
  background:green;
  color:white;
  cursor: pointer;
}
input {
  padding:0.5em;
  font-size:20px;
}
</style>
