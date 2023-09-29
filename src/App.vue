<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const number = ref(0)


function mainGameLoop(){
  const mainGameLoop = setInterval(() => {
  seepHellishEnergy();
}, 1000);
}

onMounted(){
  mainGameLoop
}

function seepHellishEnergy(){
  number += 1
}




</script>









<template>
    <p>{{ number }}</p>
    <button @click="number++">Click</button>
</template>




  <script setup>
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  
  const intervalId = ref(null);
  
  // Function to run at a regular interval
  function runFunction() {
    // Your code here
    console.log('Function executed at an interval');
    intervalId.value = requestAnimationFrame(runFunction);
  }
  
  // Start the interval when the component is mounted
  onMounted(() => {
    intervalId.value = requestAnimationFrame(runFunction);
  });
  
  // Stop the interval when the component is unmounted to prevent memory leaks
  onBeforeUnmount(() => {
    cancelAnimationFrame(intervalId.value);
  });
  </script>
  









<style scoped>
</style>
