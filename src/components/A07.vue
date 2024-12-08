<template>
  Result sum: {{ sum.toLocaleString() }}
  <br/>
  Result sum with concatenation: {{ sumWithConcatenation.toLocaleString() }}
</template>

<script setup lang="ts">
import input from './A07.input';

const equations = input.split(`\n`);

const getPossibleResults = (possibleCurrentTerms: number[], nextTerms: number[], useConcatenation = false) => {
  if (!nextTerms.length) {
    return possibleCurrentTerms;
  }

  const possibleResults = [];
  for (const possibleCurrentTerm of possibleCurrentTerms) {
    possibleResults.push(possibleCurrentTerm + nextTerms[0]);
    possibleResults.push(possibleCurrentTerm * nextTerms[0]);

    if (useConcatenation) {
      possibleResults.push(parseInt('' + possibleCurrentTerm + nextTerms[0]), 10);
    }
  }

  return getPossibleResults(possibleResults, nextTerms.slice(1), useConcatenation);
}

let sum = 0;
let sumWithConcatenation = 0;
equations.forEach((equation) => {
  const targetResult = parseInt(equation.split(':')[0]);
  const terms = equation.split(' ').slice(1).map((term) => parseInt(term, 10));

  const possibleResults = getPossibleResults([terms[0]], terms.slice(1));
  if (possibleResults.includes(targetResult)) {
    sum += targetResult;
  }

  // -----

  const possibleResultsWithConcatenation = getPossibleResults([terms[0]], terms.slice(1), true);
  if (possibleResultsWithConcatenation.includes(targetResult)) {
    sumWithConcatenation += targetResult;
  }
});
</script>
