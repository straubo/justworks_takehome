<script setup>
</script>

<template>
  <div class="marqueeContainer">
    <div v-for="n in 4">
      <div class="marquee">
        <div class="track">
          <div class="infiniteMarquee">
            &nbsp;Crypto Exchange Calculator Crypto Exchange Calculator Crypto Exchange Calculator Crypto Exchange Calculator
          </div>
        </div>
      </div>
      <div class="marquee">
        <div class="trackSlower">
          <div class="infiniteMarquee">
            &nbsp;Crypto Exchange Calculator Crypto Exchange Calculator Crypto Exchange Calculator Crypto Exchange Calculator
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .marqueeContainer {
    background-color: rgb(37, 45, 66);
    position: absolute;
    z-index: -2;
    margin: 0;
    padding: 0;
    color: rgba(132, 132, 132, 0.635);
    font-size: 144px;
    font-weight: bold;
    left: 0;
    top: 0;
    height: 100vh;
    overflow-y: hidden;
  }
  .marquee {
    position: relative;
    width: 100vw;
    max-width: 100%;
    height: 200px;
    overflow-x: hidden;
    overflow-y: hidden;
  }
  .track {
    position: absolute;
    white-space: nowrap;
    will-change: transform;
    animation: marquee 32s linear infinite;
  }
  .trackSlower {
    position: absolute;
    white-space: nowrap;
    will-change: transform;
    animation: marqueeReversed 70s linear infinite;
  }
  @keyframes marquee {
    from { transform: translateX(0); }
    to { transform: translateX(-50%); }
  }
  @keyframes marqueeReversed {
    from { transform: translateX(0); }
    to { transform: translateX(-50%); }
  }
  </style>
