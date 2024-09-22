<template>
  <div class="px-2">
    <h1 class="is-size-1 mt-6 has-text-centered">Haha Play the GAME!!!!</h1>
    
    <div class="has-text-centered my-2">
      <h3 class="is-size-3">Time Remaining</h3>
      <p class="is-size-2 has-text-weight-bold" 
        :class="10000/1000 <= (elapsed / 1000) ? 'has-text-danger' : '' " >{{ ((15000/1000) - (elapsed / 1000)).toFixed(1).replace("-", "") }}s</p>
    </div>

    <div class="columns is-centered is-vcentered">
 

      <div class="column has-text-centered has-background-success has-text-white">
        
        <div class="m-6">
           <h4 class="is-size-4">
              Correct
            </h4>
           <p class="is-size-2 has-text-weight-bold">
              {{howManyCorrect}}
           </p>
        </div>
      </div>
      <div class="column is-4 ">
        <div class="gameInterface box">
        <h2 class="is-size-2 has-text-centered my-4">
          <span class="mr-2 has-text-weight-bold">{{solutionOne}}</span>
          <span>{{props.selectedOperator}}</span>
          <span class="ml-2 has-text-weight-bold">{{solutionTwo}}</span>
          <span class="ml-2 has-text-weight-bold" v-if="outOfTime == true || isCorrect == true || isIncorrect == true">
            = {{answer}}
          </span>
        </h2>
        <div class="field has-addons has-addons-centered is-expanded" v-if="outOfTime == false && isCorrect == false && isIncorrect == false">
          <p class="control">
            <a class="button is-large is-danger" @click="userInput--">
              -
            </a>
          </p>
          <p class="control">
            <input class="input is-large has-text-centered" size="3" @keypress.enter="submitAnswer" v-model="userInput">
          </p>
          <p class="control">
            <a class="button is-large is-info" @click="userInput++">
              +
            </a>
          </p>
        </div>

        <div class="has-text-centered my-4" v-else >
          <h2 v-if="isCorrect == true" class="has-text-success is-size-4"> You got it correct!!! </h2>
          <h2 v-if="isIncorrect == true" class="has-text-danger is-size-4"> You got it wrong!!! </h2>
          <h2 v-if="outOfTime == true" class="has-text-danger is-size-4">You took to long, pathetic!!! </h2>
        </div>

        </div>
      </div>

      <div class="column has-text-centered has-background-danger has-text-white">
        <div class="m-6">
           <h4 class="is-size-4">
              Incorrect
            </h4>
           <p class="is-size-2 has-text-weight-bold">
              {{howManyIncorrect}}
           </p>
        </div>
     </div>

    </div>
  
    <div class="buttons is-centered" >
      <button class="button is-success is-rounded" v-if="outOfTime == false && isCorrect == false && isIncorrect == false" @click="submitAnswer">Submit</button>
      <button class="button is-info is-rounded" v-if="outOfTime == true " @click="setAnswer">Next Question</button>
      <button class="button is-danger is-rounded" @click="$emit('quitGame', [howManyIncorrect, howManyCorrect])">Quit</button>
    </div>


  </div>
</template>

<script setup>
  import { ref, defineProps, onMounted, onUnmounted, computed } from "vue"
  
  let lastTime
  let handle
  
  const update = () => {
    elapsed.value = performance.now() - lastTime
    if (elapsed.value >= duration.value) {
      cancelAnimationFrame(handle)
      outOfTime.value = true
      howManyIncorrect.value++
    } else {
      handle = requestAnimationFrame(update)
    }
  }

  const reset = () => {
    outOfTime.value = false
    elapsed.value = 0
    lastTime = performance.now()
    update()
  }
  const props = defineProps(['selectedNumberRange', 'selectedOperator'])
  

  const duration = ref(15 * 1000)
  const elapsed = ref(0)
  

  const answer = ref(0)
  const userInput = ref(0)
  const solutionOne = ref(0)
  const solutionTwo = ref(0)

  const howManyCorrect = ref(0)
  const howManyIncorrect = ref(0)
  
  const isCorrect = ref(false)
  const isIncorrect = ref(false)
  const outOfTime = ref(false)

  let setAnswer = () => {
    
    reset()
    isCorrect.value = false
    isIncorrect.value = false
    solutionOne.value = Math.floor(Math.random() * props.selectedNumberRange)
    
    solutionTwo.value = Math.floor(Math.random() * props.selectedNumberRange)
    let operators = {
      '+': solutionOne.value + solutionTwo.value,
      '-': solutionOne.value - solutionTwo.value,
      '*': solutionOne.value * solutionTwo.value,
      '/': solutionOne.value / solutionTwo.value
    }
    answer.value = operators[props.selectedOperator]
    console.log(answer.value, solutionOne.value, solutionTwo.value)
  }

  let submitAnswer = () => {
    userInput.value == answer.value ? inputIsCorrect() : inputIsIncorrect()
    cancelAnimationFrame(handle)
    setTimeout(() => {
      setAnswer()
    }, 1500)
  }

  let inputIsCorrect = () => {
    isCorrect.value = true
    howManyCorrect.value++
    console.log("You got it right")
  }

  let inputIsIncorrect = () => {
    isIncorrect.value = true
    howManyIncorrect.value++
    console.log("You got it wrong")
  }

  onMounted(() => {
    setAnswer()
  })
  onUnmounted(() => {
    cancelAnimationFrame(handle)
  })


</script>

<style scoped>
.gameInterface {
  min-height:200px;
}

</style>
