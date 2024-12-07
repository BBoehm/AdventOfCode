<template>
  XMAS count: {{ count.toLocaleString() }}
  <br/>
  X-MAS count {{ crossCount.toLocaleString() }}
</template>

<script setup lang="ts">
import input from './A04.input';

const splitInput = input.split('\n').map(part => part.split(''));

const checkHorizontalForwards = (i: number, j: number) => {
  if (j > splitInput[i].length - 4) {
    return false;
  }

  return (
    splitInput[i][j] === 'X' &&
    splitInput[i][j + 1] === 'M' &&
    splitInput[i][j + 2] === 'A' &&
    splitInput[i][j + 3] === 'S'
  );
}

const checkHorizontalBackwards = (i: number, j: number) => {
  if (j > splitInput[i].length - 4) {
    return false;
  }

  return (
    splitInput[i][j] === 'S' &&
    splitInput[i][j + 1] === 'A' &&
    splitInput[i][j + 2] === 'M' &&
    splitInput[i][j + 3] === 'X'
  );
}

const checkVerticalForwards = (i: number, j: number) => {
  if (i > splitInput.length - 4) {
    return false;
  }

  return (
    splitInput[i][j] === 'X' &&
    splitInput[i + 1][j] === 'M' &&
    splitInput[i + 2][j] === 'A' &&
    splitInput[i + 3][j] === 'S'
  );
}

const checkVerticalBackwards = (i: number, j: number) => {
  if (i > splitInput.length - 4) {
    return false;
  }

  return (
    splitInput[i][j] === 'S' &&
    splitInput[i + 1][j] === 'A' &&
    splitInput[i + 2][j] === 'M' &&
    splitInput[i + 3][j] === 'X'
  );
}

const checkDiagonalBottomRight = (i: number, j: number) => {
  if (i > splitInput.length - 4 || j > splitInput[i].length - 4) {
    return false;
  }

  return (
    splitInput[i][j] === 'X' &&
    splitInput[i + 1][j + 1] === 'M' &&
    splitInput[i + 2][j + 2] === 'A' &&
    splitInput[i + 3][j + 3] === 'S'
  );
}

const checkDiagonalBottomLeft = (i: number, j: number) => {
  if (i > splitInput.length - 4 || j < 3) {
    return false;
  }

  return (
    splitInput[i][j] === 'X' &&
    splitInput[i + 1][j - 1] === 'M' &&
    splitInput[i + 2][j - 2] === 'A' &&
    splitInput[i + 3][j - 3] === 'S'
  );
}


const checkDiagonalTopLeft = (i: number, j: number) => {
  if (i > splitInput.length - 4 || j > splitInput[i].length - 4) {
    return false;
  }

  return (
    splitInput[i][j] === 'S' &&
    splitInput[i + 1][j + 1] === 'A' &&
    splitInput[i + 2][j + 2] === 'M' &&
    splitInput[i + 3][j + 3] === 'X'
  );
}

const checkDiagonalTopRight = (i: number, j: number) => {
  if (i > splitInput.length - 4 || j < 3) {
    return false;
  }

  return (
    splitInput[i][j] === 'S' &&
    splitInput[i + 1][j - 1] === 'A' &&
    splitInput[i + 2][j - 2] === 'M' &&
    splitInput[i + 3][j - 3] === 'X'
  );
}

let counter = 0;

const increaseCountOnCondition = (condition: boolean) => {
  if (condition) {
    counter ++;
  }
}

for (let i = 0; i < splitInput.length; i++) {
  for (let j = 0; j < splitInput[i].length; j++) {
    [
      checkHorizontalForwards,
      checkHorizontalBackwards,
      checkVerticalForwards,
      checkVerticalBackwards,
      checkDiagonalBottomRight,
      checkDiagonalTopLeft,
      checkDiagonalBottomLeft,
      checkDiagonalTopRight,
    ].forEach((callback) => {
      increaseCountOnCondition(callback(i,j))
    });
  }
}

const count = counter;

// -----

counter = 0;
const checkCross = (i: number, j: number) => {
  if (i === 0 || j === 0 || i === splitInput.length - 1 || j === splitInput[i].length - 1) {
    return false;
  }

  if (splitInput[i][j] !== 'A') {
    return false;
  }

  const bottomRightCorrect = (splitInput[i - 1][j - 1] === 'M') && (splitInput[i + 1][j + 1] === 'S')
    || (splitInput[i - 1][j - 1] === 'S') && (splitInput[i + 1][j + 1] === 'M');

  const bottomLeftCorrect = (splitInput[i + 1][j - 1] === 'M') && (splitInput[i - 1][j + 1] === 'S')
    || (splitInput[i + 1][j - 1] === 'S') && (splitInput[i - 1][j + 1] === 'M');

  return (
    bottomRightCorrect &&
    bottomLeftCorrect
  );
}

for (let i = 0; i < splitInput.length; i++) {
  for (let j = 0; j < splitInput[i].length; j++) {
    increaseCountOnCondition(checkCross(i,j))
  }
}

const crossCount = counter;
</script>
