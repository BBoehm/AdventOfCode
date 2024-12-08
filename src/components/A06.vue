<template>
  Count of positions visited: {{ count.toLocaleString() }}
  <br/>
  Possible positions for loop obstacle: {{ loopCount.toLocaleString() }}
</template>

<script setup lang="ts">
import input from './A06.input';

let matrix = input.split('\n').map((row) => row.split(''));

const directions = ['^', '>', 'v', '<'];
let facingDirection = '^';

let currentX = 0;
let currentY = 0;

let gone = false;
let isInLoop = false;
let loopCount = 0;

loop1: for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[i].length; j++) {
    if (directions.includes(matrix[i][j])) {
      currentX = j;
      currentY = i;
      break loop1; // small performance optimization
    }
  }
}

let startX = currentX;
let startY = currentY;


const turn = () => {
  const index = directions.findIndex((direction) => facingDirection === direction);
  // noinspection UnnecessaryLocalVariableJS
  const newDirection = directions[(index + 1) % 4];
  facingDirection = newDirection;
}

const isFacingUp = () => facingDirection === '^';
const isFacingRight = () => facingDirection === '>';
const isFacingDown = () => facingDirection === 'v';
const isFacingLeft = () => facingDirection === '<';

const isObstacleInFront = () => {
  if (currentY === 0 || currentY === matrix.length - 1 || currentX === 0 || currentX === matrix[currentY].length - 1) {
    return false;
  }

  return isFacingUp() && matrix[currentY - 1][currentX] === '#'
    || isFacingRight() && matrix[currentY][currentX + 1] === '#'
    || isFacingDown() && matrix[currentY + 1][currentX] === '#'
    || isFacingLeft() && matrix[currentY][currentX - 1] === '#';
};

let positionsVisited: string[] = [];
let positionsVisitedWithDirection: string[] = [];

const step = () => {
  matrix[currentY][currentX] = 'X';
  if (!positionsVisited.includes(`${currentX},${currentY}`)) {
    positionsVisited.push(`${currentX},${currentY}`);
  }

  if (positionsVisitedWithDirection.includes(`${currentX},${currentY},${facingDirection}`)) {
    isInLoop = true;
    loopCount++;
  }

  positionsVisitedWithDirection.push(`${currentX},${currentY},${facingDirection}`);


  if (isFacingUp()) {
    currentY--;
  }

  if (isFacingRight()) {
    currentX++;
  }

  if (isFacingDown()) {
    currentY++;
  }

  if (isFacingLeft()) {
    currentX--;
  }

  if (currentY < 0 || currentY === matrix.length || currentX < 0 || currentX === matrix[currentY].length) {
    gone = true;
    return;
  }

  matrix[currentY][currentX] = facingDirection;

}

while (!gone) {
  while (!gone && !isObstacleInFront()) {
    step();
  }

  turn();
}

const count = positionsVisited.length;

// -----

positionsVisited.forEach((positionVisited) => {
  // reset
  matrix = input.split('\n').map((row) => row.split(''));
  currentX = startX;
  currentY = startY;
  facingDirection = '^';
  gone = false;
  isInLoop = false;
  positionsVisitedWithDirection = [];

  const visitedX = parseInt(positionVisited.split(',')[0], 10);
  const visitedY = parseInt(positionVisited.split(',')[1], 10);

  if (visitedX === startX && visitedY === startY) {
    return;
  }

  // set new obstacle
  matrix[visitedY][visitedX] = '#';

  while (!gone && !isInLoop) {
    while (!gone && !isInLoop && !isObstacleInFront()) {
      step();
    }

    turn();
  }
});

</script>
