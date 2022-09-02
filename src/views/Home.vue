<script setup>
import Calculator from "../components/Calculator/Calculator.vue";
import { calculatorModes } from "../components/Calculator/config.js";
import Switch from "../components/Switch.vue";
import { ref } from "vue";

const calculatorMode = ref(calculatorModes.default);

const onCalculatorModeChange = () =>
  calculatorMode.value === calculatorModes.expression
    ? (calculatorMode.value = calculatorModes.default)
    : (calculatorMode.value = calculatorModes.expression);
</script>

<template>
  <main>
    <div class="calculator-wrapper">
      <div class="calculator-mode-switch-wrapper">
        <Switch
          :checked="calculatorMode === calculatorModes.expression ? true : false"
          :onChange="onCalculatorModeChange"
        />
        <h3>
          {{
            calculatorMode === calculatorModes.expression ? "Expression mode" : "Default"
          }}
        </h3>
      </div>
      <Calculator :mode="calculatorMode" />
    </div>
  </main>
</template>

<style>
main {
  min-height: 100vh;
  margin: 4rem 0;
}

.calculator-mode-switch-wrapper {
  margin-bottom: 1rem;
  align-self: flex-start;
  display: flex;
  gap: 10px;
  align-items: center;
  color: white;
}

.calculator-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: fit-content;
  margin: auto;
}
</style>
