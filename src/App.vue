<script setup>
import { ref } from 'vue';
import AssetCalculator from './components/calculator.vue'
import WelcomePane from './components/welcomePane.vue'
import marqueeContainer from './components/marqueeContainer.vue';
let appEntered = ref(false)
</script>

<template>
  <marqueeContainer />
  <WelcomePane
    @enterClicked="(input) => appEntered = input"
    v-if="!appEntered"
  />
  <AssetCalculator
    v-else
  />
</template>

<style scoped>
</style>
