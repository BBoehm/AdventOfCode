<template>
  Multiplication sum: {{sumOfMultiplication.toLocaleString()}}
  <br/>
  Cleaned multiplication sum: {{sumOfCleanedMultiplication.toLocaleString()}}
</template>

<script setup lang="ts">
import input from './A03.input';

const matchRegex = /mul\(\d{1,3},\d{1,3}\)/gm;
const matches = input.match(matchRegex)!;

// noinspection JSUnusedGlobalSymbols
const mul = (a: number, b: number) => a * b;

const sumOfMultiplication = matches.reduce((sum: number, match: string) => {
  return sum + eval(match);
}, 0);

// -----

const cleanedInput = input.replaceAll('\n', '').replaceAll(/don't\(\)(.*?)do\(\)/gm, '').replaceAll(/don't\(\)(.*)/gm, '');
const cleanedMatches = cleanedInput.match(matchRegex)!;

const sumOfCleanedMultiplication = cleanedMatches.reduce((sum: number, match: string) => {
  return sum + eval(match);
}, 0);
</script>
