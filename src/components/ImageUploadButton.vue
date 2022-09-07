<script setup>
const { id, onChange } = defineProps(["id", "onChange"]);

</script>
<template>
  <input
    type="file"
    name="image"
    value=""
    :id="id"
    class="image-upload-input"
    accept="image/png, image/jpeg, image/bmp, image/pbm"
    @change="onChange"
  />
  <label
    :for="id"
    v-html="'&#128247;'"
    class="image-upload-label custom-button bg-color-secondary"
  ></label>
</template>
<style>
.image-upload-label {
  font-size: 2em;
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
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.bg-color-secondary {
  background-color: var(--secondary);
}
.bg-color-secondary:hover {
  background-color: var(--secondary-hover);
}
</style>
