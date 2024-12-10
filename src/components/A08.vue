<template>
  Count of antiNodes: {{count.toLocaleString()}}
  <br/>
  Count of antiNodes with resonant harmonics: {{resonantHarmonicsCount.toLocaleString()}}
</template>

<script setup lang="ts">
import input from './A08.input';

const matrix = input.split('\n').map((row) => row.split(''));

const antennaTypes = Array.from(new Set(input)).filter((char) => !['\n', '.'].includes(char));

const antennas: string[] = [];

for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[i].length; j++) {
    const char = matrix[i][j];

    if (!['\n', '.'].includes(char)) {
      antennas.push(`${j},${i},${char}`);
    }
  }
}

const antiNodes: string[] = [];
const resonantHarmonicsAntiNodes: string[] = [];

const isInBounds = (x: number, y: number) => {
  return x >= 0 && y >= 0 && x < matrix[0].length && y < matrix.length
}

const getAntiNodes = (x1: number, x2: number, y1: number, y2: number) => {
  const deltaX = x2 - x1;
  const deltaY = y2 - y1;

  return [
    ...(isInBounds(x2 + deltaX, y2 + deltaY) ? [`${x2 + deltaX},${y2 + deltaY}`] : []),
    ...(isInBounds(x1 - deltaX, y1 - deltaY) ? [`${x1 - deltaX},${y1 - deltaY}`] : []),
  ]
}

const getResonantHarmonicsAntiNodes = (x1: number, x2: number, y1: number, y2: number) => {
  const deltaX = x2 - x1;
  const deltaY = y2 - y1;

  let xToCheck = x2;
  let yToCheck = y2;
  const antiNodes = [];

  while(isInBounds(xToCheck, yToCheck)) {
    antiNodes.push(`${xToCheck},${yToCheck}`);

    xToCheck += deltaX;
    yToCheck += deltaY;
  }

  xToCheck = x1;
  yToCheck = y1;

  while(isInBounds(xToCheck, yToCheck)) {
    antiNodes.push(`${xToCheck},${yToCheck}`);

    xToCheck -= deltaX;
    yToCheck -= deltaY;
  }

  return antiNodes;
}

antennaTypes.forEach((antennaType) => {
  const antennasOfType = antennas.filter((antenna) => antenna.split(',')[2] === antennaType);

  for (let i = 1; i < antennasOfType.length; i++) {
    for (let j = 0; j < i; j++) {
      const x1 = parseInt(antennasOfType[i].split(',')[0], 10);
      const y1 = parseInt(antennasOfType[i].split(',')[1], 10);
      const x2 = parseInt(antennasOfType[j].split(',')[0], 10);
      const y2 = parseInt(antennasOfType[j].split(',')[1], 10);

      const newAntiNodes = getAntiNodes(x1, x2, y1, y2);
      const newResonantHarmonicsAntiNodes = getResonantHarmonicsAntiNodes(x1, x2, y1, y2);

      newAntiNodes.forEach((antiNode) => {
        if (!antiNodes.includes(antiNode)) {
          antiNodes.push(antiNode);
        }
      })

      // -----

      newResonantHarmonicsAntiNodes.forEach((antiNode) => {
        if (!resonantHarmonicsAntiNodes.includes(antiNode)) {
          resonantHarmonicsAntiNodes.push(antiNode);
        }
      })
    }
  }
});

const count = antiNodes.length;

// -----

const resonantHarmonicsCount = resonantHarmonicsAntiNodes.length;


</script>
