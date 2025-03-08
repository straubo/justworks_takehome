<script setup>
import {ref, watch} from 'vue'
const emit = defineEmits(['enterClicked'])
</script>

<template>
  <div class="welcomePane">
    <div class="welcomeHeader">Crypto Portfolio Calculator</div>
    <button @click="emit('enterClicked', true)">Enter</button>
  </div>
</template>

<style scoped>
  .welcomePane {
    width: 100%;
  }
  .welcomeHeader {
    font-size: 64px;
  }
  button {
    background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
    height: 10vh;
    width: 45%;
    margin-top: 36px;
  }
  @keyframes gradient {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }
</style>
