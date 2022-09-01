<script setup>
const props = defineProps(["backgroundColor", "symbol"]);

const backgroundColor = props.backgroundColor;
const symbol = props.symbol;
</script>
<template>
  <div class="custom-button" :class="backgroundColor">
    <h1 class="custom-button-symbol">
      {{ symbol }}
    </h1>
  </div>
</template>
<style>
.custom-button {
  width: 75px;
  height: 75px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.custom-button-symbol {
    color: white;
}

.green {
  background-color: #1cc883;
}

.grey {
  background-color: #35425f;
}
</style>
