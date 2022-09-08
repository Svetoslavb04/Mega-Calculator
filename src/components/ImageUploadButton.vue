<script setup>
const { id, onChange, classCSS } = defineProps(["id", "onChange, classCSS"]);
</script>
<template>
  <input
    type="file"
    name="image"
    value=""
    :id="id"
    class="image-upload-input"
    accept="image/jpeg, image/jpg"
    @change="onChange"
  />
  <label
    :for="id"
    v-html="'&#128247;'"
    class="image-upload-label custom-button bg-color-secondary"
    :class="classCSS"
  ></label>
</template>
<style>
.image-upload-label {
  font-size: 2.6em;
}

.image-upload-input {
  width: 0.1px;
  height: 0.1px;
  opacity: 0;
  overflow: hidden;
  position: absolute;
  z-index: -1;
}

.custom-button {
  width: 75px;
  height: 75px;
  border-radius: 5px;
  display: flex;
  align-items: baseline;
  justify-content: center;
  cursor: pointer;
}

.bg-color-secondary {
  background-color: var(--secondary);
}
.bg-color-secondary:hover {
  background-color: var(--secondary-hover);
}

@media (max-width: 500px) {
  .image-upload-label {
    font-size: 2em;
  }
  .custom-button {
    width: 55px;
    height: 60px;
  }
}

@media (max-width: 650px) {
  .expression-mode .custom-button {
    width: 55px;
    height: 60px;
  }

  .expression-mode .image-upload-label {
    font-size: 2em;
  }
}
</style>
