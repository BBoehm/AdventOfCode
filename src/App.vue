<template>
  <main>
    <section>
      <ul>
        <li v-for="index in 25">
          <button @click="() => {
            error = '';
            currentIndex = padIndex(index);
            importFile();
          }">
            #{{padIndex(index)}}
          </button>
        </li>
      </ul>
    </section>

    <section>
      <div v-if="error">
        {{ error}}
      </div>

      <component v-else :is="currentFile" />
    </section>
  </main>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';

const currentIndex = ref('01')
const currentFile = ref(null);
const error = ref('');
const padIndex = (index: number) => String(index).padStart(2, '0');
const importFile = async () => {
  try {
    currentFile.value = (await import(`./components/A${currentIndex.value}.vue`)).default;
  } catch (e: Error) {
    if (e.message.includes('Unknown variable dynamic import')) {
      error.value = 'This has not been implemented yet';
    } else {
      error.value = e.message;
    }
  }
}

onMounted(async () => {
  await importFile();
})

</script>

<style>
main {
  display: flex;
}

section {
  display: flex;
  align-items: center;
  flex-grow: 1;
}

button {
  background: white;
  border: none;
  color: blue;
  text-decoration: underline;
}

li {
  list-style: none;
}
</style>
