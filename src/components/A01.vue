<template>
  Total Diff: {{totalDiff.toLocaleString()}}
  <br/>
  Similarity: {{similarity.toLocaleString()}}
</template>

<script setup lang="ts">
import input from './A01.input';

const splitInput = input.split('\n');

let string0 = '';
let string1 = '';

splitInput.map((currentPair: string) => {
  const values = currentPair.split('   ');
  string0 = string0.concat(' ', values[0]);
  string1 = string1.concat(' ', values[1]);
});

const array0 = string0.split(' ').filter(x => x).sort();
const array1 = string1.split(' ').filter(x => x).sort();

let totalDiff = 0;

for (let i = 0; i < array0.length; i++) {
  const diff = Math.abs(parseInt(array0[i], 10) - parseInt(array1[i], 10));
  totalDiff += diff;
}

// -----

let startOfInnerLoop = 0;
let similarity = 0;

for (let i = 0; i < array0.length; i++) {
  const baseValue = parseInt(array0[i], 10);
  let matchCount = 0;
  let compareValue = 0;

  for (let j = startOfInnerLoop; (compareValue = parseInt(array1[j])) <= baseValue; j++) {
    compareValue = parseInt(array1[j]);
    if (compareValue === baseValue) {
      matchCount++;
    } else {
      // count up startOfInnerLoop for performance optimization
      startOfInnerLoop++;
    }
  }

  similarity += baseValue * matchCount;
}
</script>
