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
    height: 60vh;
    background-color: rgba(205, 205, 205, 0.769);
    border: 1px solid black;
    border-radius: 8px;
    padding: 8px;
    @media (max-width: 500px) {
      width: 94%;
    }
  }
  .welcomeHeader {
    font-size: 64px;
    @media (max-width: 500px) {
      margin-top: 24px;
      font-size: 32px;
    }
  }
  button {
    background: linear-gradient(-45deg, #c16f56, #a72b2d, #6899ab, #4fa75c);
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
    height: 10vh;
    width: 45%;
    margin-top: 20%;
    @media (max-width: 500px) {
      margin-top: 40%;
    }
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
