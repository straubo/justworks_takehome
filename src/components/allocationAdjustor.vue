<script setup>
import { ref } from 'vue'

const props = defineProps({
  exchangeData: Object,
  currencies: Array,
  allocations: Array
})
const emit = defineEmits(['xButtonClicked', 'updateCurrency'])

function updateCurrency(index, event) {
  const newCurrency = event.target.value
  emit('updateCurrency', { index, newCurrency })
}
</script>

<template>
  <div class="overlayDiv">
    <div class="assetReallocationContainer">
      <button class="xButton" @click="emit('xButtonClicked')">X</button>
      <div class="title">Asset Reallocation Pane</div>
      <div class="subtext">
        Feel free to change which currencies you're investing in here.
      </div>
      <div class="currencyAdjustmentsContainer">
        <div class="currencyAdjustmentContainer">
          <div>{{ currencies[0] }} Allocation: {{ allocations[0] }}</div>
          <select @change="updateCurrency(0, $event)">
            <option 
              v-for="item in Object.keys(props.exchangeData.data.rates)" 
              :key="item" 
              :value="item"
            >
              {{ item }}
            </option>
          </select>
        </div>
        <div class="currencyAdjustmentContainer">
          <div>{{ currencies[1] }} Allocation: {{ allocations[1] }}</div>
          <select @change="updateCurrency(1, $event)">
            <option 
              v-for="item in Object.keys(props.exchangeData.data.rates)" 
              :key="item"
              :value="item"
            >
              {{ item }}
            </option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
  .overlayDiv {
    position: absolute;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(117, 117, 117, 0.726);
  }
  .assetReallocationContainer {
    position: absolute;
    width: 32%;
    z-index: 10;
    left: 34%;
    top: 30vh;
    min-height: 45vh;
    background-color: rgba(212, 212, 212, 0.826);
    border: 1px solid black;
    border-radius: 4px;
    padding: 16px;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
    @media (max-width: 500px) {
      width: 80%;
      left: 6%;
      top: 15vh;
    }
  }
  .xButton {
    position: absolute;
    width: 36px;
    height: 36px;
    left: 10px;
    top: 10px;
    border: 2px solid black;
    border-radius: 50%;
    background-color: white;
    padding: 0;
  }
  .assetReallocationContainer .title {
    font-size: 36px;
    @media (max-width: 500px) {
      font-size: 24px;
    }
  }
  .subtext {
    width: 75%;
    font-style: italic;
  }
  .currencyAdjustmentsContainer {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    width: 80%;
    @media (max-width: 500px) {
      width: 90%;
    }
  }
  .currencyAdjustmentContainer {
    display: inline-block;
    border: 1px solid black;
    border-radius: 4px;
    padding: 4px;
  }
  .rangeContainer {
    display: block;
  }
  .rangeContainer input {
    margin-top: 8px;
  }
</style>