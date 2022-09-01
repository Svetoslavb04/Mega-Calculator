<script setup>
const props = defineProps(["backgroundColor", "symbol"]);

const backgroundColor = props.backgroundColor;
const symbol = props.symbol;
</script>
<template>
  <div class="custom-button" :class="`bg-color-${backgroundColor}`">
    <h1 class="custom-button-symbol">
      {{ symbol }}
    </h1>
  </div>
</template>
<style scoped>

.custom-button {
  width: 75px;
  height: 75px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.custom-button-symbol {
    color: white;
}

.bg-color-green {
  background-color: #1cc883;
}

.bg-color-grey {
  background-color: #35425f;
}
</style>
