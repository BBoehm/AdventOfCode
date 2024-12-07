<template>
  Number of save reports: {{saveReportCount.toLocaleString()}}
  <br/>
  Number of save reports with dampening: {{saveReportCountWithDampening.toLocaleString()}}
</template>

<script setup lang="ts">
import input from './A02.input';

const reports = input.split('\n');
let saveReportCount = 0;

const isUnsafe = (isAscending: boolean, currentLevel: number, nextLevel: number) => {
  if (isAscending && (nextLevel < currentLevel)) { // not all increasing
    return true;
  }

  if (!isAscending && (nextLevel > currentLevel)) { // not all decreasing
    return true;
  }

  if (nextLevel === currentLevel) { // distance is 0
    return true;
  }

  // noinspection RedundantIfStatementJS
  if (Math.abs(nextLevel - currentLevel) > 3) { // distance > 3
    return true;
  }

  return false;
}

const someLevelsUnsafe = (levels: string[]) => {
  const isAscending = parseInt(levels[1], 10) > parseInt(levels[0], 10);
  for (let i = 0; i < levels.length - 1; i++) {
    const currentLevel = parseInt(levels[i], 10);
    const nextLevel = parseInt(levels[i + 1], 10);

    if (isUnsafe(isAscending, currentLevel, nextLevel)) {
      return i;
    }
  }

  return false;
}

const dampenLevel = (levels: string[], unsafeLevelIndex: number) => levels.filter((level, index) => index !== unsafeLevelIndex);

reports.forEach((report) => {
  const levels = report.split(' ');

  if (someLevelsUnsafe(levels) !== false) {
    return;
  }

  saveReportCount++;
});

// -----

let saveReportCountWithDampening = 0;
reports.forEach((report) => {
  const levels = report.split(' ');

  const unsafeLevelIndex = someLevelsUnsafe(levels);

  if (unsafeLevelIndex !== false) {
    if(
      someLevelsUnsafe(dampenLevel(levels, unsafeLevelIndex)) !== false
      && someLevelsUnsafe(dampenLevel(levels, unsafeLevelIndex + 1)) !== false
      && someLevelsUnsafe(dampenLevel(levels, unsafeLevelIndex - 1)) !== false
    ) {
      // still unsafe after dampening
      return;
    }
  }

  saveReportCountWithDampening++;
});
</script>
