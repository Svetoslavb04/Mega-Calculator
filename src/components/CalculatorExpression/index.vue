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

const { result } = defineProps(["result"]);
const isMobileViewEnabled = ref(window.innerWidth <= 650);
const isProcessingImage = ref(false);

window.addEventListener(
  "resize",
  () => (isMobileViewEnabled.value = window.innerWidth <= 650)
);

const trigonometricSymbols = ["sin", "cos", "tg", "cotg"];

const math = create(all, {
  number: "BigNumber",
  precision: 11,
});

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
        type: result.type,
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
          isProcessingImage.value = false;
          result.value = "Error";
        });
    },
    error() {
      isProcessingImage.value = false;
      result.value = "Error";
    },
  });
}

const onButtonClick = (symbol) => {
  if (symbol === "C") {
    return (result.value = "0");
  }

  if (symbol === "📷") {
      return;
    }

 if(symbol === "CE" ){
   return clearElement();
  }

  const prevSymbol = result.value[result.value.length-1];
  if (operationSymbols.includes(prevSymbol) && operationSymbols.includes(symbol)) {
    if (symbol === "=") {
      return;
    }else return result.value = result.value.replace(prevSymbol, symbol);
   
   } 

  if (symbol === "x<sup>y</sup>") { 
    if (operationSymbols.includes(prevSymbol)) {
      result.value = result.value.replace(prevSymbol, "^"); 
      return;
    }else return result.value += "^";
  }

  if (symbol === "=") {
    replacePercentageExpression();
    replaceTgAndCotg();
    replaceSqrt();

    return (result.value = `${math.evaluate(result.value)}`);
  }

  if (result.value === "0" && !operationSymbols.includes(symbol)) {
    return (result.value = symbol);
  }


  const f = resultContainsTrigonometricSymbol();
  if (f!= -1) {
    if (operationSymbols.includes(prevSymbol) && operationSymbols.include(symbol)) {
      return result.value = result.value.replace(f, "");
    }else return result.value = result.value.replace(f, symbol);
  }
  

  const expressionParts = result.value.split(" ");
  const lastExpressionPart = expressionParts[expressionParts.length - 1];

  if (operationSymbols.some((symbol) => !lastExpressionPart.includes(symbol))) {
    result.value += symbol;
  } else {
    result.value += " " + symbol;
  }

  
};

function clearElement(){
   if (result.value.length>1) {
      let characters = -1;

      if (result.value[result.value.length-1] === 'n') {
         characters = -3;
      }else if (result.value[result.value.length-1] === 's') {
        characters = -3;
      }else if (result.value[result.value.length-3] === 'o') {
        characters = -4;
      }else if (result.value[result.value.length-1] === 'g') {
        characters = -2;
      }

      if (result.value.length + characters <= 0) {
        return result.value = "0";
      } else return result.value = result.value.slice(0, characters); 

    }else return result.value = "0";
}

function resultContainsTrigonometricSymbol(){
  let trSymbol = -1;
   trigonometricSymbols.forEach(element => {
    if (result.value.substring(result.value.length-4).includes(element)) {
      trSymbol = element; 
    }
  });

  return trSymbol;
}

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

function replaceSqrt() {
  const sqrtRegex = /√\(.+\)/g;
  let match = result.value.match(sqrtRegex);
  if (match) {
    match.forEach((element) => {
      const number = element.substring(1);
      result.value = result.value.replace(`√${number}`, `sqrt(${number})`);
    });
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

function onCameraButtonClick(e) {
  const data = new FormData();
  data.append("locale", "en");
  data.append("image", e.target.files[0], "Untitled.jpg");

  const options = {
    method: "POST",
    headers: {
      "X-RapidAPI-Key": "c400b29832msh6666338ddbcfb72p1bf952jsn8aaf15ca7975",
      "X-RapidAPI-Host": "photomath1.p.rapidapi.com",
    },
    body: data,
  };

  fetch("https://photomath1.p.rapidapi.com/maths/solve-problem", options)
    .then((res) => res.json())
    .then((res) => {

      let solution = res.result.groups[0].entries[0].preview.content.solution;

      while (true) {
        if (solution.children) {
          solution = solution.children[solution.children.length - 1];
        } else {
          break;
        }
      }

      result.value = solution.value;

    })
    .catch((err) => {
      result.value = "Error";
    });
}
</script>
<template>
  <div
    v-if="isMobileViewEnabled"
    id="calculator-expression-mobile"
    class="calculator-buttons-wrapper"
  >
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
      <CustomButton
        v-if="isProcessingImage"
        :component="Spinner"
        componentClass="image-loading-spinner"
        backgroundColor="secondary"
        :onButtonClick="onButtonClick"
      />
      <ImageUploadButton
        classCSS="image-upload-label"
        v-else
        id="image-upload"
        :onChange="onCameraButtonClick"
      />
    </div>
    <div class="buttons-row">
      <CustomButton
        style="width: 100%"
        symbol="="
        backgroundColor="primary"
        :onButtonClick="onButtonClick"
      />
    </div>
    <div class="buttons-row">
      <CustomButton
        v-for="data in [
          { symbol: '(', bgColor: 'primary' },
          { symbol: ')', bgColor: 'primary' },
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
          { symbol: 'sin', bgColor: 'primary' },
          { symbol: 'cos', bgColor: 'primary' },
          { symbol: 'tg', bgColor: 'primary' },
          { symbol: 'cotg', bgColor: 'primary' },
        ]"
        :backgroundColor="data.bgColor"
        :symbol="data.symbol"
        :onButtonClick="onButtonClick"
      />
    </div>
  </div>
  <div v-else id="calculator-expression" class="calculator-buttons-wrapper">
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
<style scoped>
@media (max-width: 650px) {
  .custom-button {
    width: 55px;
    height: 60px;
    align-items: center;
  }
}
</style>
