<script setup>
import { ref } from "vue";
import { create, all } from "mathjs";
import Compressor from "compressorjs";

import {
  calculatorModes,
  specialSymbols,
  calculateableSymbols,
  operationSymbols,
} from "../Calculator/config.js";

import CustomButton from "../../components/CustomButton.vue";
import ImageUploadButton from "../../components/ImageUploadButton.vue";
import Spinner from "../../components/Spinner.vue";

const math = create(all, {
  number: "BigNumber",
  precision: 11,
});

const { result } = defineProps(["result"]);

const isProcessingImage = ref(false);

function onButtonClick(symbol) {
  const operation = operationSymbols.filter((symbol) =>
    result.value.slice(1).includes(symbol)
  )[0];

  if (
    (["", "0", "Error", "📷"].includes(result.value) &&
      operationSymbols.some((opSymbol) =>
        opSymbol === "-" ? false : opSymbol === symbol
      )) ||
    symbol === "📷" ||
    (result.value[result.value.length - 1] === "." && symbol === ".")
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
    const numbers = result.value.split(" ");

    if (!isNaN(numbers[0]) && !isNaN(numbers[2])) {
      return handleOperation(operation);
    }

    if (operation) {
      return (result.value += " " + symbol);
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

  try {
    result.value = `${math.evaluate(result.value)}`;
  } catch (error) {
    result.value = "Error";
  }
}

function onCameraButtonClick(e) {
  const file = e.target.files[0];

  if (!file) {
    return;
  }

  new Compressor(file, {
    quality: 0.2,
    convertSize: 100,
    maxWidth: 500,
    success(resultBlob) {
      
      const newFile = new File([resultBlob], "image.jpg", {
        type: result.type
      });

      const data = new FormData();
      data.append("locale", "en");
      data.append("image", newFile);

      const options = {
        method: "POST",
        headers: {
          "X-RapidAPI-Key": "c400b29832msh6666338ddbcfb72p1bf952jsn8aaf15ca7975",
          "X-RapidAPI-Host": "photomath1.p.rapidapi.com",
        },
        body: data,
      };

      isProcessingImage.value = true;

      fetch("https://photomath1.p.rapidapi.com/maths/solve-problem", options)
        .then((res) => res.json())
        .then((res) => {

          if (res.message) {
            throw res.message;
          }

          let solution = res.result.groups[0].entries[0].preview.content.solution;

          while (true) {
            if (solution.children) {
              solution = solution.children[solution.children.length - 1];
            } else {
              break;
            }
          }
          
          result.value = solution.value;
          isProcessingImage.value = false;
        })
        .catch((err) => {
          result.value = "Error";
        });
    },
  });
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
        ]"
        :backgroundColor="data.bgColor"
        :symbol="data.symbol"
        :onButtonClick="onButtonClick"
      />
      <div>
        <CustomButton
          v-if="isProcessingImage"
          :component="Spinner"
          componentClass="image-loading-spinner"
          backgroundColor="secondary"
          :onButtonClick="onButtonClick"
        />
        <ImageUploadButton v-else id="image-upload" :onChange="onCameraButtonClick" />
      </div>
      <CustomButton symbol="=" backgroundColor="primary" :onButtonClick="onButtonClick" />
    </div>
  </div>
</template>
<style>
.image-loading-spinner {
  width: 63px;
  height: 63px;
}

.image-loading-spinner div {
  border-width: 6px;
  width: 48px;
  height: 48px;
}
</style>
