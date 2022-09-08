<script setup>
const {
  prbackgroundColorops,
  symbol,
  onButtonClick,
  component,
  componentClass,
} = defineProps(["backgroundColor", "symbol", "onButtonClick", "component", "componentClass"]);
</script>
<template>
  <div
    class="custom-button"
    :class="`bg-color-${backgroundColor}`"
    @click="onButtonClick(symbol)"
  >
    <component v-if="component" :is="component" :classCSS="componentClass"></component>
    <h1 v-if="symbol" v-html="symbol" class="custom-button-symbol"></h1>
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
.bg-color-primary:hover {
  background-color: var(--primary-hover);
}

.bg-color-secondary {
  background-color: var(--secondary);
}
.bg-color-secondary:hover {
  background-color: var(--secondary-hover);
}
</style>
