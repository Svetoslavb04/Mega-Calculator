<script setup>
import { create, all } from "mathjs";

import {
  calculatorModes,
  specialSymbols,
  calculateableSymbols,
  operationSymbols,
} from "../Calculator/config.js";

import CustomButton from "../../components/CustomButton.vue";

const math = create(all, {
  number: "BigNumber",
  precision: 11,
});

const { result } = defineProps(["result"]);

function onButtonClick(symbol) {
  const operation = operationSymbols.filter((symbol) => result.value.includes(symbol))[0];

  if (
    (result.value === "" || result.value === "0" || result.value === "Error") &&
    operationSymbols.some((opSymbol) => opSymbol === symbol)
  ) {
    return;
  }

  if (symbol === "C") {
    result.value = "0";
  } else if (result.value === "0" || result.value === "Error") {
    result.value = symbol;
  } else if (operationSymbols.some((opSymbol) => opSymbol === symbol) && operation) {
    handleOperation(operation);
  } else if (
    operationSymbols.some((symbol) => result.value.includes(symbol)) &&
    operationSymbols.includes(symbol)
  ) {
    handleOperation(symbol);
  } else if (symbol === "=") {
    return;
  } else {
    result.value += symbol;
  }
}

function handleOperation(operation) {
  const numbers = result.value.split(operation).filter(Boolean);

  if (numbers.length < 2) {
    return (result.value = "Error");
  }

  if (operation !== "%") {
    result.value = `${math.evaluate(`${numbers[0]} ${operation} ${numbers[1]}`)}`;
  } else {
    result.value = `${math.evaluate(`${numbers[0]}${operation} * ${numbers[1]}`)}`;
  }
}
</script>
<template lang="">
    <div id="calculator-default" class="calculator-buttons-wrapper">
      <div class="buttons-row">
        <CustomButton
          v-for="data in [
            { symbol: 'C', bgColor: 'primary' },
            { symbol: '*', bgColor: 'primary' },
            { symbol: '/', bgColor: 'primary' },
            { symbol: '-', bgColor: 'primary' },
          ]"
          :backgroundColor="data.bgColor"
          :symbol="data.symbol"
          :onButtonClick="onButtonClick"
        />
      </div>
      <div class="buttons-row">
        <CustomButton
          v-for="data in [
            { symbol: '7', bgColor: 'secondary' },
            { symbol: '8', bgColor: 'secondary' },
            { symbol: '9', bgColor: 'secondary' },
            { symbol: '+', bgColor: 'primary' },
          ]"
          :backgroundColor="data.bgColor"
          :symbol="data.symbol"
          :onButtonClick="onButtonClick"
        />
      </div>
      <div class="buttons-row">
        <CustomButton
          v-for="data in [
            { symbol: '4', bgColor: 'secondary' },
            { symbol: '5', bgColor: 'secondary' },
            { symbol: '6', bgColor: 'secondary' },
            { symbol: '%', bgColor: 'primary' },
          ]"
          :backgroundColor="data.bgColor"
          :symbol="data.symbol"
          :onButtonClick="onButtonClick"
        />
      </div>
      <div class="buttons-row">
        <CustomButton
          v-for="data in [
            { symbol: '1', bgColor: 'secondary' },
            { symbol: '2', bgColor: 'secondary' },
            { symbol: '3', bgColor: 'secondary' },
            { symbol: '=', bgColor: 'primary' },
          ]"
          :backgroundColor="data.bgColor"
          :symbol="data.symbol"
          :onButtonClick="onButtonClick"
        />
      </div>
      <div class="buttons-row">
        <CustomButton
          v-for="data in [
            { symbol: '0', bgColor: 'secondary' },
            { symbol: '.', bgColor: 'secondary' },
          ]"
          :backgroundColor="data.bgColor"
          :symbol="data.symbol"
          :onButtonClick="onButtonClick"
        />
      </div>
    </div> 
</template>
<style lang="">
    
</style>