<script setup>
import { ref, watch } from 'vue'
import allocationAdjustor from './allocationAdjustor.vue'

// init variables, etc
const assetValue = ref(null)
const incomingConversionData = ref(null)
const currencies = ref(["BTC", "ETH"])
const conversionRates = ref([0, 0])
const finalConversions = ref([0, 0])
const allocations = ref([.7, .3])
const inputLabel = ref("Investable Funds (USD)")
const adjustingAllocations = ref(false)

const updatedInputLabel = "Calculate another amount (in USD)"

const haveCalculated = ref(false)

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
  conversionRates.value[0] = incomingConversionData.value.data.rates[currencies.value[0]]
  conversionRates.value[1] = incomingConversionData.value.data.rates[currencies.value[1]]
}

watch(incomingConversionData, updateConversionRates)

// use converstion data to calculate end values
function calculateValues() {
  haveCalculated.value = true
  const btcAllocation = assetValue.value * allocations.value[0]
  const ethAllocation = assetValue.value * allocations.value[1]

  finalConversions.value[0] = btcAllocation * conversionRates.value[0]
  finalConversions.value[1] = ethAllocation * conversionRates.value[1]
  inputLabel.value = updatedInputLabel
  // console.log(incomingConversionData.value.data.rates[currencies.value[0]])
  // console.log(incomingConversionData.value.data.rates[currencies.value[1]])
}

</script>

<template>
  <div class="appContainer">
    <div class="header">
      Asset Allocation Calculator
    </div>
    <div class="formContainer">
      <TransitionGroup 
        name="inputContainer" 
        tag="div"
        class="inputContainer"
      >
        <div
          class="calcTitle"
          key="0"
        >
          {{ inputLabel }}</div>
        <input 
          v-model="assetValue" 
          placeholder="Funds"
          type="number"
          :key="1"
        >
        <button 
          class="calculateButton" 
          @click="calculateValues"
          v-if="assetValue"
          :key="2"
        >Calculate</button>
      </TransitionGroup>
      <div
        class="outputContainer"
        v-if="haveCalculated"
      >
        <div class="valuesHeader">Distributions</div>
        <div class="valueContainer">
          <div class="calcTitle">70% BTC Allocation</div>
          <div class="calcValue">
            {{ finalConversions[0] || "no input yet" }}
          </div>
        </div>
        <div class="valueContainer">
          <div class="calcTitle">30% ETH Allocation</div>
          <div class="calcValue">
            {{ finalConversions[1] || "no input yet" }}
          </div>
        </div>
        <button
          class="adjustButton"
        >Adjust Positions</button>
        <allocationAdjustor
          v-if="adjustingAllocations"
          :exchangeData="incomingConversionData"
          :currencies="currencies"
          :conversionRates="conversionRates"
          :allocations="allocations"
        />
      </div>
    </div>
    <div class="disclaimer" v-if="haveCalculated">
      *With the volatile nature of crypto markets/pricing in mind, these calculations are approximate.
      Please leave room for fluctuations, surcharges, or other surprises. This is not financial advice.
    </div>
  </div>
</template>

<style scoped>

.appContainer {
  width: 90%;
  max-width: 928px;
  height: 80vh;
  background-color: rgba(194, 194, 194, 0.678);
  margin: 0 auto;
  padding: 36px;
  font-family: system-ui, Avenir, Helvetica, Arial, sans-serif;
  border: 1px solid black;
  border-radius: 8px;
  display: flex;
    flex-direction: column;
  @media (max-width: 500px) {
    width: 98%;
    padding: 4px;
    height: auto;
    max-height: 90vh;
    overflow-y: scroll;
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
  margin: 30px auto 0px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  width: 75%;
  @media (max-width:500px) {
    width: 95%;
    flex-direction: column;
  }
}

.inputContainer {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  /* height: 15vh; */
}

.inputContainer input {
  display: block;
  height: 32px;
  text-align: center;
  border-radius: 4px;
}

.inputContainer input[type="number"]::-webkit-inner-spin-button,
.inputContainer input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.inputContainer-move,
.inputContainer-enter-active,
.inputContainer-leave-active {
  transition: all 0.5s ease;
}
.inputContainer-enter-from,
.inputContainer-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.inputContainer-leave-active {
  position: absolute;
}


.calculateButton {
  background-color: white;
  border-radius: 4px;
  /* width: 100%; */
  /* this ^^ fixed positioning issue, but created a
  sizing issue -refine if we have time */
  @media (max-width: 500px) {
    margin-top: 8px;
  }
}

.formCalculator {
  display: flex;
  flex-direction: row;
}

.calcValue {
  width: 100%; 
  height: 36px;
  padding-top: 12px;
  margin-top: 8px;
  text-align: center;
  border: 1px solid black;
  border-radius: 5px;
}

.outputContainer {

}

.valuesHeader {
  font-size: 48px;
}
.valueContainer {
  margin-top: 30px;
}

.calcTitle {
  font-weight: 700;
}

.adjustButton {
  margin-top: 15px;
  background-color: white;
}
.disclaimer {
  /* margin: 36px 0 0 16px; */
  margin: 10vh auto;
  font-style: italic;
  width: 40%;
  font-size: 16px;
  border: 1px solid black;
  border-radius: 4px;
  padding: 4px;
  @media  (max-width: 500px) {
    width: 90%;
    margin: 30px auto 0;
    font-size: 12px;
  }
}
</style>
