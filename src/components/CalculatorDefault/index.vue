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
  const operation = operationSymbols.filter((symbol) =>
    result.value.slice(1).includes(symbol)
  )[0];

  if (
    (["", "0", "Error", "📷"].includes(result.value) &&
      operationSymbols.some((opSymbol) =>
        opSymbol === "-" ? false : opSymbol === symbol
      )) ||
    symbol === "📷"
  ) {
    return;
  }

  if (symbol === "C") {
    return (result.value = "0");
  }

  if (symbol === "CE") {
    if (result.value.length > 1 && !["Error", "Infinity"].includes(result.value)) {
      result.value = result.value.slice(0, -1);
    } else result.value = "0";

    return;
  }

  if (result.value === "0" || result.value === "Error") {
    return (result.value = symbol);
  }

  if (symbol === "-") {
    
    const numbers = result.value.split(' ');
    
    if (!isNaN(numbers[0]) && !isNaN(numbers[2])) {
      return handleOperation(operation);
    }

    if (operation) {
      return result.value += ' ' + symbol;
    }
    
    if (result.value.length === 1 && ["0", "-"].includes(result.value)) {
      return;
    }
  }

  if (operationSymbols.some((opSymbol) => opSymbol === symbol) && operation) {
    return handleOperation(operation);
  }
  
  if (
    operationSymbols.some((symbol) => result.value.slice(1).includes(symbol)) &&
    operationSymbols.includes(symbol)
  ) {
    return handleOperation(symbol);
  }

  if (symbol === "=") {
    return;
  }
  
  if (
    (calculateableSymbols.includes(result.value[result.value.length - 1]) &&
      calculateableSymbols.includes(symbol)) ||
    (result.value[result.value.length - 1] === "-" && result.value[0] === "0") ||
    (result.value[0] === "-" && result.value.length === 1) ||
    (result.value.indexOf(operation) != result.value.length - 1 && !isNaN(symbol))
  ) {
    result.value += symbol;
  } else {
    result.value += " " + symbol;
  }
}

function handleOperation(operation) {
  const numbers = result.value.split(operation).filter(Boolean);

  if (numbers.length < 2) {
    return (result.value = "Error");
  }

  const number_1 = numbers[0].trim();
  const number_2 = numbers[1].trim();

  result.value = result.value.replace(
    `${number_1} % ${number_2}`,
    `${number_1}% * ${number_2}`
  );
  console.log(result.value);

  result.value = `${math.evaluate(result.value)}`;
}
</script>
<template>
  <div id="calculator-default" class="calculator-buttons-wrapper">
    <div class="buttons-row">
      <CustomButton
        v-for="data in [
          { symbol: 'C', bgColor: 'primary' },
          { symbol: 'CE', bgColor: 'primary' },
          { symbol: '*', bgColor: 'primary' },
          { symbol: '/', bgColor: 'primary' },
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
          { symbol: '4', bgColor: 'secondary' },
          { symbol: '5', bgColor: 'secondary' },
          { symbol: '6', bgColor: 'secondary' },
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
          { symbol: '1', bgColor: 'secondary' },
          { symbol: '2', bgColor: 'secondary' },
          { symbol: '3', bgColor: 'secondary' },
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
          { symbol: '0', bgColor: 'secondary' },
          { symbol: '.', bgColor: 'secondary' },
          { symbol: '&#128247;', bgColor: 'secondary' },
          { symbol: '=', bgColor: 'primary' },
        ]"
        :backgroundColor="data.bgColor"
        :symbol="data.symbol"
        :onButtonClick="onButtonClick"
      />
    </div>
  </div>
</template>
<style lang=""></style>
