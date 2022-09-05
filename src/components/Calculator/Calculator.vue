<script setup>
import { ref } from "vue";

import {
  calculatorModes,
  specialSymbols,
  calculateableSymbols,
  operationSymbols,
} from "./config.js";

import CustomButton from "../../components/CustomButton.vue";
import CalcualtorDefault from "../../components/CalcualtorDefault/index.vue";

const props = defineProps(["mode"]);

const result = ref({
  value: "0"
});

</script>
<template>
  <div
    id="calculator"
    :class="{ 'expression-mode': props.mode === calculatorModes.expression }"
  >
    <div class="input-box-wrapper">
      <input type="text" name="input-box" id="input-box" :value="result.value" disabled />
    </div>
    <CalcualtorDefault :result="result" v-if="props.mode === calculatorModes.default" />
    <div
      id="calculator-expression"
      v-if="props.mode === calculatorModes.expression"
      class="calculator-buttons-wrapper"
    >
      <div class="buttons-row">
        <CustomButton
          backgroundColor="primary"
          symbol="C"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="*"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="/"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="-"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="("
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol=")"
          :onButtonClick="onButtonClick"
        />
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
        <CustomButton
          backgroundColor="primary"
          symbol="+"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="x<sup>y</sup>"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="√"
          :onButtonClick="onButtonClick"
        />
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
        <CustomButton
          backgroundColor="primary"
          symbol="%"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="sin"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="cos"
          :onButtonClick="onButtonClick"
        />
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
        <CustomButton
          backgroundColor="primary"
          symbol="="
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="tg"
          :onButtonClick="onButtonClick"
        />
        <CustomButton
          backgroundColor="primary"
          symbol="cotg"
          :onButtonClick="onButtonClick"
        />
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
::selection {
  color: #3b3939;
  background: #d1cdcd;
}
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
  user-select: none;
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
