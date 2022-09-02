<script setup>
import CustomButton from "../../components/CustomButton.vue";
import { ref } from "vue";
import { create, all } from "mathjs";
import { calculatorModes } from "./config.js";

const props = defineProps(["mode"]);

const math = create(all, {
  number: "BigNumber",
});

const specialSymbols = ["C", "="];
const calculateableSymbols = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "."];
const operationSymbols = ["*", "/", "%", "-", "+", "="];

const result = ref("0");

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
<template>
  <div
    id="calculator"
    :class="props.mode === calculatorModes.expression ? 'expression-mode' : ''"
  >
    <div class="input-box-wrapper">
      <input type="text" name="input-box" id="input-box" :value="result" disabled />
    </div>
    <div v-if="props.mode === calculatorModes.default" class="calculator-buttons-wrapper">
      <div class="buttons-row">
        <CustomButton backgroundColor="primary" symbol="C" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="*" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="/" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="-" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="7"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="8"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="9"
          :onButtonClick="onButtonClick"
        />
        <CustomButton backgroundColor="primary" symbol="+" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="4"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="5"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="6"
          :onButtonClick="onButtonClick"
        />
        <CustomButton backgroundColor="primary" symbol="%" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="1"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="2"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="3"
          :onButtonClick="onButtonClick"
        />
        <CustomButton backgroundColor="primary" symbol="=" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="0"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="."
          :onButtonClick="onButtonClick"
        />
      </div>
    </div>
    <div
      v-if="props.mode === calculatorModes.expression"
      class="calculator-buttons-wrapper"
    >
      <div class="buttons-row">
        <CustomButton backgroundColor="primary" symbol="C" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="*" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="/" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="-" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="(" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol=")" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="7"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="8"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="9"
          :onButtonClick="onButtonClick"
        />
        <CustomButton backgroundColor="primary" symbol="+" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="x<sup>y</sup>" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="√" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="4"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="5"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="6"
          :onButtonClick="onButtonClick"
        />
        <CustomButton backgroundColor="primary" symbol="%" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="sin" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="cos" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="1"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="2"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="3"
          :onButtonClick="onButtonClick"
        />
        <CustomButton backgroundColor="primary" symbol="=" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="tg" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="primary" symbol="cotg" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton
          backgroundColor="secondary"
          symbol="0"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="secondary"
          symbol="."
          :onButtonClick="onButtonClick"
        />
        
        <CustomButton
          backgroundColor="secondary"
          symbol="&#128247;"
          :onButtonClick="onButtonClick"
        />
      </div>
    </div>
  </div>
</template>
<style>
:root {
  --calculator-width: 400px;
}

#calculator.expression-mode {
  --calculator-width: 600px;
}

#calculator {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 20px;
  border-radius: 5px;
  width: var(--calculator-width);
  height: 550px;
  background-color: #252f45;
}

.calculator-buttons-wrapper {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.buttons-row {
  display: flex;
  gap: 20px;
}

.buttons-row:last-child {
  justify-content: flex-start;
}

.input-box-wrapper {
  width: 100%;
}

#input-box {
  width: 100%;
  padding: 12px;
  height: 80px;
  background-color: var(--secondary) !important;
  border: 2px solid var(--primary);
  border-radius: 4px;
  font-size: 1.5rem;
  color: white;
  cursor: pointer;
}

#input-box:focus-visible {
  outline: none;
  border: 2px solid var(--primary) !important;
}
</style>
