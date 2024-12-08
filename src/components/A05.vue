<template>
  Sum of middle pages of correct updates: {{ sumOfMiddlePagesOfCorrectUpdates.toLocaleString() }}
  <br/>
  Sum of middle pages of fixed incorrect updates: {{ sumOfMiddlePagesOfFixedUpdates.toLocaleString() }}
</template>

<script setup lang="ts">
import input from './A05.input';

const rules = input.rules;
const updates = input.updates.split('\n');

const isPageOrderCorrect = (page1: string, page2: string) => {
  return !rules.includes(`${page2}|${page1}`);
}

const isUpdateOrderCorrect = (update: string) => {
  const pages = update.split(',');
  for (let i = 0; i < pages.length; i++) {
    for (let j = i; j < pages.length; j++) {
      if(!isPageOrderCorrect(pages[i],pages[j])) {
        return false;
      }
    }
  }

  return true;
}

const correctUpdates = updates.filter((update) => isUpdateOrderCorrect(update));

const getSumOfMiddlePages = ((updates: string[]) => updates.reduce((sum: number, update) => {
  const pagesInUpdate = update.split(',');
  return sum + parseInt(pagesInUpdate[(pagesInUpdate.length - 1) / 2], 10);
}, 0))

const sumOfMiddlePagesOfCorrectUpdates = getSumOfMiddlePages(correctUpdates);

// -----

const incorrectUpdates = updates.filter((update) => !isUpdateOrderCorrect(update));
const fixUpdateOrder = (update: string) => {
  while (!isUpdateOrderCorrect(update)) {
    const pages = update.split(',');
    for (let i = 0; i < pages.length; i++) {
      for (let j = i; j < pages.length; j++) {
        if(!isPageOrderCorrect(pages[i],pages[j])) {
          // swap pages
          [pages[i], pages[j]] = [pages[j], pages[i]];
          update = pages.join(',');
        }
      }
    }
  }

  return update;
}

incorrectUpdates[2] = fixUpdateOrder(incorrectUpdates[2])
const fixedUpdates = incorrectUpdates.map((update) => fixUpdateOrder(update));

const sumOfMiddlePagesOfFixedUpdates = getSumOfMiddlePages(fixedUpdates);
</script>
