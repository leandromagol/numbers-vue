<script setup lang="ts">
import { ref, computed } from 'vue';

const limit = ref(100);
const hovNumber = ref(null);

//using computed to generate the numbers for better reactivity
const n = computed(() => {
  const numbers = Array.from({ length: limit.value }, (_, i) => i + 1);
  return [...numbers].sort(() => Math.random() - 0.5);
});

//Using pure functions to check if the number is a divisor of hovNumber
const isDivisor = (number) => {
  if (!hovNumber.value) return false;
  return hovNumber.value % number === 0;
};


//Using vue to mannage reactivity instead of manipulating the dom directly  
function hov(number) {
  hovNumber.value = number;
};


function reset() {
  hovNumber.value = null;
};
</script>

<template>
  <div>
    <input type="number" v-model.number="limit" min="1" max="1000" /><br /><br />
    <div v-for="number in n" :key="number" :id="'number-' + number" class="number"
      :class="{ 'active': isDivisor(number) }" @mouseover="hov(number)" @mouseout="reset">
      {{ number }}
    </div>
  </div>
</template>

<style>
.number {
  display: inline-block;
  padding: 5px;
  background-color: lightgrey;
  margin: 5px;
}

.active {
  background-color: red;
}
</style>
