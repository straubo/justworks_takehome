<script setup>
import { ref, watch } from 'vue';
import AssetCalculator from './components/calculator.vue'
import WelcomePane from './components/welcomePane.vue'
let appEntered = ref(false)
watch(appEntered, () => {console.log("this has been entered", appEntered.value)})
</script>

<template>
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
