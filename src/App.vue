<template>
  <div
    class="container mx-auto text-center text-teal-600 px-12 flex content-center justify-center flex-nowrap flex-col mt-8"
  >
    <h1 class="text-4xl font-black">Password Trainer</h1>
    <div class="text-gray-800 my-4">
      <p>Train your fingers muscle memory on your new password.</p>
    </div>
    <PassWordTrainer />
  </div>
</template>
<script setup>
import PassWordTrainer from "./components/PassWordTrainer.vue";
</script>
<style>
body {
  @apply bg-gray-100;
}
</style>
