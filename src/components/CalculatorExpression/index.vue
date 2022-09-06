<script setup>
import { create, all } from "mathjs";

import {
  calculatorModes,
  specialSymbols,
  calculateableSymbols,
  operationSymbols,
} from "../Calculator/config.js";

import CustomButton from "../../components/CustomButton.vue";

const { result } = defineProps(["result"]);

const math = create(all, {
  number: "BigNumber",
  precision: 11
});

const onButtonClick = (symbol) => {
  if (symbol === "C") {
    return (result.value = "0");
  }

 if(symbol === "CE" ){
   if (result.value.length>1) {
      return result.value = result.value.slice(0,-1);
    }else return result.value = "0";
  }

  if (symbol === "=") {
    replacePercentageExpression();
    replaceTgAndCotg();

    return (result.value = `${math.evaluate(result.value)}`);
  }

  if (result.value === "0" && !operationSymbols.includes(symbol)) {
    return (result.value = symbol);
  }

  const expressionParts = result.value.split(" ");
  const lastExpressionPart = expressionParts[expressionParts.length - 1];

  if (operationSymbols.some((symbol) => !lastExpressionPart.includes(symbol))) {
    result.value += symbol;
  } else {
    result.value += " " + symbol;
  }
};

function replacePercentageExpression() {
  const percentageRegex = /[0-9]+ % [0-9]+/g;
  let matchIndex = result.value.search(percentageRegex);

  while (matchIndex >= 0) {
    const stringStartingWithMatch = result.value.substring(matchIndex);
    const [number_1, _, number_2] = stringStartingWithMatch.split(" ");

    result.value = result.value.replace(
      `${number_1} % ${number_2}`,
      `${number_1}% * ${number_2}`
    );

    matchIndex = result.value.search(percentageRegex);
  }
}

function replaceTgAndCotg() {
  const tgRegex = /\btg\([0-9a-zA-Z()*\/+\-%]+\)/g;
  let matchIndex = result.value.search(tgRegex);

  while (matchIndex >= 0) {
    const stringStartingWithMatch = result.value.substring(matchIndex);

    const tgExpression = stringStartingWithMatch.substring(
      3,
      stringStartingWithMatch.length - 1
    );

    result.value = result.value.replace(`tg(${tgExpression})`, `tan(${tgExpression})`);

    matchIndex = result.value.search(tgRegex);
  }

  const cotgRegex = /cotg\([0-9a-zA-Z()*\/+\-%]+\)/g;
  matchIndex = result.value.search(cotgRegex);
  console.log(result.value);

  while (matchIndex >= 0) {
    const stringStartingWithMatch = result.value.substring(matchIndex);

    const cotgExpression = stringStartingWithMatch.substring(
      5,
      stringStartingWithMatch.length - 1
    );

    result.value = result.value.replace(
      `cotg(${cotgExpression})`,
      `cot(${cotgExpression})`
    );

    matchIndex = result.value.search(cotgRegex);
  }
}
</script>
<template>
  <div id="calculator-expression" class="calculator-buttons-wrapper">
    <div class="buttons-row">
      <CustomButton
        v-for="data in [
          { symbol: 'C', bgColor: 'primary' },
          { symbol: 'CE', bgColor: 'primary' },
          { symbol: '*', bgColor: 'primary' },
          { symbol: '/', bgColor: 'primary' },
          { symbol: '(', bgColor: 'primary' },
          { symbol: ')', bgColor: 'primary' },
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
          { symbol: 'x<sup>y</sup>', bgColor: 'primary' },
          { symbol: '√', bgColor: 'primary' },
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
          { symbol: 'sin', bgColor: 'primary' },
          { symbol: 'cos', bgColor: 'primary' },
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
          { symbol: 'tg', bgColor: 'primary' },
          { symbol: 'cotg', bgColor: 'primary' },
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
<style></style>
