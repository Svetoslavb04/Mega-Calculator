<script setup>
import CustomButton from "../components/CustomButton.vue";
import { ref } from "vue";

const result = ref("");

function onButtonClick(symbol) {

  const specialSymbols = ["C", "*", "/", "%", "=", "-", "+"];

  if (!specialSymbols.includes(symbol)) {
    result.value += symbol;
  }

  if (symbol === "C") {
    result.value = "";
  } else {
  }
}
</script>

<template>
  <main>
    <div id="calculator">
      <div class="input-box-wrapper">
        <input type="text" name="input-box" id="input-box" :value="result" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="green" symbol="C" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="green" symbol="*" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="green" symbol="/" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="green" symbol="-" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="secondary" symbol="7" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="8" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="9" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="green" symbol="+" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="secondary" symbol="4" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="5" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="6" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="green" symbol="%" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="secondary" symbol="1" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="2" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="3" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="green" symbol="=" :onButtonClick="onButtonClick" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="secondary" symbol="0" :onButtonClick="onButtonClick" />
        <CustomButton backgroundColor="secondary" symbol="." :onButtonClick="onButtonClick" />
      </div>
    </div>
  </main>
</template>

<style>
main {
  min-height: 100vh;
  margin: 4rem 0;
  display: flex;
  justify-content: center;
}

#calculator {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 20px;
  border-radius: 5px;
  width: 400px;
  height: 550px;
  background-color: #252f45;
}

.buttons-row {
  display: flex;
  justify-content: space-between;
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
  border: 1px solid var(--primary);
  border-radius: 4px;
  font-size: 1.5rem;
  color: white;
}

#input-box:focus-visible {
  outline: none;
  border: 2px solid var(--primary) !important;
}
</style>
