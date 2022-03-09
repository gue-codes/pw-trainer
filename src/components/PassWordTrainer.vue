<template>
  <div class="flex flex-col space-y-4">
    <input
      :type="editing ? 'text' : 'password'"
      v-model="password"
      class="bg-gray-200 mx-auto text-center px-4 py-2 font-mono text-black text-xl rounded"
      placeholder="Enter password here"
      :disabled="editing === false"
    />
    <button
      v-if="editing === true && password.length > 3"
      class="rounded border mx-auto px-4 py-2 font-bold border-teal-600 hover:bg-teal-600 hover:text-white"
      @click="switchEditMode"
    >
      Start Training
    </button>
    <button
      v-if="editing === false"
      class="rounded border mx-auto px-4 py-2 font-bold border-teal-600 hover:bg-teal-600 hover:text-white"
      @click="resetGame"
    >
      End Training
    </button>
    <div
      v-if="editing === false"
      class="flex flex-row space-x-2 justify-center items-center justify-center"
    >
      <div
        v-for="(char, index) in pwSpread"
        :key="`char-${index}`"
        class="bg-gray-200 text-gray-900 font-bold w-12 h-12 rounded flex content-center items-center justify-center text-white mt-8"
        :class="addCheckClass(index)"
      >
        {{ glimpse === true ? char : "•" }}
      </div>
    </div>
    <div v-if="editing === false">
      <p class="font-bold m-4">Enter your Guess and hit ENTER</p>
      <input
        :type="glimpse === false ? 'password' : 'text'"
        class="bg-white mx-auto text-center px-4 py-2 font-mono text-black text-xl rounded"
        placeholder="Type here"
        v-model="finalGuess"
        @keyup.enter="checkGuess"
        autofocus
      />
      <div
        class="m-6 text-gray-500 bg-white border border-teal-500 rounded shadow-md max-w-lg mx-auto flex content-center justify-around items-center flex-nowrap"
      >
        <div class="font-bold text-xl my-3 p-1 text-center mx-2">
          Your statistic
        </div>
        <p class="flex flex-col m-1 p-1 border border-gray-100 rounded w-28">
          <span class="text-xs font-bold p-1 border-b">Rounds played</span>
          <span class="p-1 text-3xl pt-2">{{ round }}</span>
        </p>
        <p class="flex flex-col m-1 p-1 border border-gray-100 rounded w-28">
          <span class="text-xs font-bold p-1 border-b">Correct</span>
          <span class="p-1 text-3xl pt-2">{{ correctGuesses }}</span>
        </p>
        <p class="flex flex-col m-1 p-1 border border-gray-100 rounded w-28">
          <span class="text-xs font-bold p-1 border-b">Wrong</span>
          <span class="p-1 text-3xl pt-2">{{ incorrectGuesses }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
const password = ref("");
const passwordGuess = ref("");
const finalGuess = ref("");
const round = ref(0);
const correctGuesses = ref(0);
const incorrectGuesses = ref(0);
const glimpse = ref(false);
let editing = ref(true);
const pwSpread = computed(function () {
  return password.value.split("");
});
const guessSpread = computed(function () {
  return passwordGuess.value.split("");
});
function switchEditMode() {
  editing.value = !editing.value;
}
function addCheckClass(index) {
  if (guessSpread.value.length > 0) {
    return pwSpread.value[index] === guessSpread.value[index]
      ? "bg-teal-500"
      : "bg-red-500";
  } else {
    return "";
  }
}
function checkGuess() {
  glimpse.value = true;
  passwordGuess.value = finalGuess.value;
  round.value++;
  if (passwordGuess.value === password.value) {
    correctGuesses.value++;
  } else {
    incorrectGuesses.value++;
  }
  setTimeout(function () {
    passwordGuess.value = "";
    finalGuess.value = "";
    glimpse.value = false;
  }, 2000);
}
function resetGame() {
  editing.value = true;
  round.value = 0;
  correctGuesses.value = 0;
  incorrectGuesses.value = 0;
}
</script>
