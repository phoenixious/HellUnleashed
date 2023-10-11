<template>

  <div id="bigDiv">
    <div id="left">
      <h3>Resources</h3>
      <ul class="resources">
        <li>
          <span >Hellish Energy: </span>
          <span class="unitSpace">
            <span >{{ myGameData.energy.amount }}</span>
            <span >{{ myGameData.energy.perSecond }} /s</span>
          </span>
        </li>
        <li>
          <span >Rift Essence: </span>
          <span class="unitSpace">
            <span >{{ myGameData.riftessence.amount }}</span>
            <span >{{ myGameData.riftessence.perSecond }} /s</span>
          </span>
        </li>
      </ul>
    </div>

    <div id="right">
      <button @click="clickHellishEnergy()">Click</button>
      <button @click="moreSpeed()">More speed! {{ myGameData.energy.moreSpeedCost }}</button>
    </div>
  </div>
</template>




<script setup>
//************************************************************************/
//setup variables
//************************************************************************/
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
  },

  riftessence: {
    amount: 0,
    perSecond: 1,
    moreSpeedCost: 10,
  }
})

//************************************************************************/
//Main Game Loop
//************************************************************************/

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



//************************************************************************/
//Game Logic
//************************************************************************/

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
    const rounded = Math.round(myGameData.value.energy.moreSpeedCost * 1.5);
    myGameData.value.energy.moreSpeedCost = rounded;
    myGameData.value.energy.perSecond += 1;
  }
}

</script>


//************************************************************************/
//CSS
//************************************************************************/
<style scoped>
#bigDiv {
  display: flex;
}

#left {
  width:400px;
  margin-left: 10px;
  margin-top: 10px;
}

#right {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  flex: 1;
  margin-left: 10px;
  margin-top: 10px;
}

.resources {
  width: 100%;
  padding: 0;
}

.resources li {
  list-style: none;
  display: flex;
  justify-content: space-between;
}

.unitSpace span + span {
  margin-left: 20px;
}

h3 {
  text-decoration: underline;
}
</style>



