<script setup>
import { ref, watch } from "vue";

import {
  calculatorModes,
  specialSymbols,
  calculateableSymbols,
  operationSymbols,
} from "./config.js";

import CustomButton from "../../components/CustomButton.vue";
import CalculatorDefault from "../CalculatorDefault/index.vue";
import CalculatorExpression from "../CalculatorExpression/index.vue";

const props = defineProps(["mode"]);

const result = ref({
  value: "0",
});

watch(
  () => props.mode,
  () => (result.value.value = "0")
);
</script>
<template>
  <div
    id="calculator"
    :class="{ 'expression-mode': props.mode === calculatorModes.expression }"
  >
    <div class="input-box-wrapper">
      <input type="text" name="input-box" id="input-box" :value="result.value" disabled />
    </div>
    <CalculatorDefault :result="result" v-if="props.mode === calculatorModes.default" />
    <CalculatorExpression
      :result="result"
      v-if="props.mode === calculatorModes.expression"
    />
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

.image-loading-spinner {
  width: 63px;
  height: 63px;
}

.image-loading-spinner div {
  border-width: 6px;
  width: 48px;
  height: 48px;
}

@media (max-width: 500px) {
  .image-loading-spinner {
    width: 55px;
    height: 55px;
  }

  .image-loading-spinner div {
    border-width: 4px;
    width: 40px;
    height: 40px;
  }
}

@media (max-width: 500px) {
  :root {
    --calculator-width: 320px;
  }
}

@media (max-width: 650px) {
  #calculator.expression-mode {
    --calculator-width: 320px;
  }
}
</style>
