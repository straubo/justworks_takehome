<script setup>
// init variables, etc
import { ref, watch } from 'vue'
const incomingConversionData = ref(null)
const btcConversionRate = ref(null)
const ethConversionRate = ref(null)
const assetValue = ref(null)
const btcConversion = ref(0)
const ethConversion = ref(0)

// conversion data fetch
async function fetchData() {
  incomingConversionData.value = null
  const res = await fetch(
    "https://api.coinbase.com/v2/exchange-rates?currency=USD"
  )
  incomingConversionData.value = await res.json()
}
fetchData()

function updateConversionRates() {
  btcConversionRate.value = incomingConversionData.value.data.rates.BTC
  ethConversionRate.value = incomingConversionData.value.data.rates.ETH
}

watch(incomingConversionData, updateConversionRates)


// use converstion data to calculate end values
function calculateValues() {
  console.log("value is changing")
  const btcAllocation = assetValue.value * .7
  const ethAllocation = assetValue.value * .3

  btcConversion.value = btcAllocation * btcConversionRate.value
  ethConversion.value = ethAllocation * ethConversionRate.value
}
// watch(assetValue, calculateValues)

</script>

<template>
  <div class="appContainer">
    <div class="header">
      Asset Allocation Calculator
    </div>
    <div class="formContainer">
      <div class="inputContainer">
        <div class="calcTitle">Investable Assets</div>
        <input v-model="assetValue" placeholder="Input fund quantity (USD)" >
        <button class="calculateButton" @click="calculateValues">calculate!</button>
      </div>
      <div class="outputContainer">
        <div class="valueContainer">
          <div class="calcTitle">70% BTC Allocation</div>
          <div class="calcValue">
            {{ btcConversion || "no input yet" }}
          </div>
        </div>
        <div class="valueContainer">
          <div class="calcTitle">30% ETH Allocation</div>
          <div class="calcValue">
            {{ ethConversion || "no input yet" }}
          </div>
        </div>
      </div>
    </div>
    <div class="disclaimer">*With the volatile nature of crypto markets/pricing in mind, these calculations are approximate. Please leave room for fluctuations, surcharges, or other surprises. This is not financial advice.</div>
  </div>
</template>

<style scoped>
.appContainer {
  width: 90%;
  max-width: 864px;
  margin: 0 auto;
  padding: 36px;
  font-family: system-ui, Avenir, Helvetica, Arial, sans-serif;
  border: 1px solid black;
  border-radius: 8px;
  @media (max-width: 500px) {
    width: 98%;
    padding: 4px;
  }
}
.header {
  font-size: 64px;
  font-weight: bold;
  @media (max-width: 500px) {
    font-size: 24px;
  }
}
.formContainer {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
  /* justify-content: center; */
  justify-content: space-around;
  width: 75%;
  @media (max-width:500px) {
    width: 95%;
  }
}
.calculateButton {
  margin-left: 10px;
  background-color: white;
  border-radius: 4px;
}
.formCalculator {
  display: flex;
  flex-direction: row;
}
.calcValue {
  width: 100%; 
  height: 36px;
  padding-top: 12px;
  text-align: center;
  border: 1px solid black;
  border-radius: 5px;
}
.disclaimer {
  margin: 36px 0 0 16px;
  font-style: italic;
  width: 40%;
  @media  (max-width: 500px) {
    width: 90%;
    margin: 30px auto 0;
  }
}
</style>
