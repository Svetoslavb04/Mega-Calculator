<script setup>

const props = defineProps(["backgroundColor", "symbol", "onButtonClick"]);

const backgroundColor = props.backgroundColor;
const symbol = props.symbol;
const onButtonClick = props.onButtonClick;

</script>
<template>
  <div
    class="custom-button"
    :class="`bg-color-${backgroundColor}`"
    @click="onButtonClick(symbol)"
  >
    <h1 v-html="symbol" class="custom-button-symbol">
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

.bg-color-primary {
  background-color: var(--primary);
}

.bg-color-secondary {
  background-color: var(--secondary);
}
</style>
