<template>
  <p>{{ myGameData.energy.amount }}</p>
  <button @click="clickHellishEnergy()">Click</button>
  <button @click="moreSpeed()">More speed! {{ myGameData.energy.moreSpeedCost }}</button>
</template>




<script setup>
//setup variables
import { ref, onMounted, onBeforeUnmount } from 'vue';

//declare previous time variable
let previousTime = 0;
//declare time accumilation Variable
let timeAccumilation = 0;


//game object
const myGameData = ref({
  energy: {
    amount: 0,
    perSecond: 1,
    perClick: 1,
    moreSpeedCost: 10,
  }
})

function mainGameLoop() {
  const intervalId = setInterval(() => {
    const currentTime = performance.now();
    console.log("current =", currentTime);

    //increment time accumilation
    timeAccumilation += currentTime - previousTime;

    // wrap game logic in if statement to run if timeAccumilation >= 1000
    if (timeAccumilation >= 1000) {
      // Update game logic here
      seepHellishEnergy();
    
      //reset time accumilation 
      timeAccumilation = 0;
    }
    
    // Update the previousTime for the next iteration
    previousTime = currentTime;
  }, 500);
  
  onBeforeUnmount(() => {
    clearInterval(intervalId);
  });
}

onMounted(() => {
  mainGameLoop();
});


function seepHellishEnergy() {
  // Calculate energy per second and add it to the energy
  myGameData.value.energy.amount += myGameData.value.energy.perSecond;
  console.log("Energy PS",myGameData.value.energy.perSecond);  
}

function clickHellishEnergy() {
  myGameData.value.energy.amount += myGameData.value.energy.perClick;
}

function moreSpeed() {
  if (myGameData.value.energy.amount >= myGameData.value.energy.moreSpeedCost) {
    myGameData.value.energy.amount -= myGameData.value.energy.moreSpeedCost;
    myGameData.value.energy.moreSpeedCost *= 2;
    myGameData.value.energy.perSecond += 1;
  }
}

</script>


