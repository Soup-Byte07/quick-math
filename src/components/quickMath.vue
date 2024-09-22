<template>
  <div>
    <div class="box quickMathBox">
      <div v-if="playingGame == false">
      <h1 class="is-size-2 has-text-centered m-2">Quick Math</h1>
      <p class="is-size-6 has-text-centered m-2">Play a game where you have to choose an answer, but be quick ;)</p>
      <h3 class="is-size-3 has-text-centered m-4">Choose an operation</h3>

      <div class="columns is-centered">
        <div class="column m-1 is-2 is-clickable has-text-centered" :class="selectedOperator == '+' ? 'has-background-success-light' : 'has-background-white-ter'" @click="selectedOperator = '+'" >
          <div class="icon is-large ">
            <svg  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title>plus</title><path d="M19,13H13V19H11V13H5V11H11V5H13V11H19V13Z" /></svg>
          </div>
        </div>

        <div class="column m-1 is-2 is-clickable has-text-centered" :class="selectedOperator == '-' ? 'has-background-success-light' : 'has-background-white-ter'" @click="selectedOperator = '-'">
          <div class="icon is-large ">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title>minus-thick</title><path d="M19,13H5V11H19V13Z" /></svg>
          </div>
        </div>
         <div class="column m-1 is-2 is-clickable has-text-centered"  :class="selectedOperator == '*' ? 'has-background-success-light' : 'has-background-white-ter'" @click="selectedOperator = '*'">
          <div class="icon is-large ">
         <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title>multiplication</title><path d="M19,6.41L17.59,5L12,10.59L6.41,5L5,6.41L10.59,12L5,17.59L6.41,19L12,13.41L17.59,19L19,17.59L13.41,12L19,6.41Z" /></svg> 
          </div>
        </div>
         <div class="column m-1 is-2 is-clickable has-text-centered"  :class="selectedOperator == '/' ? 'has-background-success-light' : 'has-background-white-ter'" @click="selectedOperator = '/'">
          <div class="icon is-large ">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title>division</title><path d="M19,13H5V11H19V13M12,5A2,2 0 0,1 14,7A2,2 0 0,1 12,9A2,2 0 0,1 10,7A2,2 0 0,1 12,5M12,15A2,2 0 0,1 14,17A2,2 0 0,1 12,19A2,2 0 0,1 10,17A2,2 0 0,1 12,15Z" /></svg> 
          </div>
        </div>
      </div>

      <div class="mt-2 has-text-centered">
        <h3 class="is-size-3">Choose a number range</h3>

        <div class="field has-addons has-addons-centered is-expanded">
          <p class="control">
            <a class="button is-danger" @click="selectedNumberRange--">
            -
            </a>
          </p>
          <p class="control">
            <input class="input" type="number" v-model="selectedNumberRange">
          </p>
          <p class="control">
            <a class="button is-info" @click="selectedNumberRange++">
              +
            </a>
          </p>
      </div>
          
      </div>
      <div class="buttons is-centered mt-4">
        <button class="button is-large is-success is-centered" @click="playingGame = true">Play</button>
      </div>
      </div>

      <div v-else>
        <PlayGame :selectedNumberRange="selectedNumberRange" :selectedOperator="selectedOperator" @quit-game="quitGame" />
      </div>
    </div>
  </div>
</template>

<script setup>
  import PlayGame from "./playGame.vue"
  import { ref } from 'vue'

  const playingGame = ref(false)
  const selectedNumberRange = ref(12)
  const selectedOperator = ref("+")

  const scores = ref([])

  let quitGame = (data) => {
    console.log(data)
    playingGame.value = false
  }

</script>

