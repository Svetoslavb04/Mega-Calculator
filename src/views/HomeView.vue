<script setup>
import CustomButton from "../components/CustomButton.vue";
</script>

<template>
  <main>
    <div id="calculator">
      <div class="input-box-wrapper">
        <input type="text" name="input-box" id="input-box" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="green" symbol="C" />
        <CustomButton backgroundColor="green" symbol="*" />
        <CustomButton backgroundColor="green" symbol="/" />
        <CustomButton backgroundColor="green" symbol="-" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="grey" symbol="7" />
        <CustomButton backgroundColor="grey" symbol="8" />
        <CustomButton backgroundColor="grey" symbol="9" />
        <CustomButton backgroundColor="green" symbol="+" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="grey" symbol="4" />
        <CustomButton backgroundColor="grey" symbol="5" />
        <CustomButton backgroundColor="grey" symbol="6" />
        <CustomButton backgroundColor="green" symbol="%" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="grey" symbol="1" />
        <CustomButton backgroundColor="grey" symbol="2" />
        <CustomButton backgroundColor="grey" symbol="3" />
        <CustomButton backgroundColor="green" symbol="=" />
      </div>
      <div class="buttons-row">
        <CustomButton backgroundColor="grey" symbol="0" />
        <CustomButton backgroundColor="grey" symbol="." />
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
  background-color: #35425f !important;
  border: 1px solid #1cc883;
  border-radius: 4px;
  font-size: 1.5rem;
  color: white;
}

#input-box:focus-visible {
  outline: none;
  border: 2px solid #1cc883 !important;
}
</style>
