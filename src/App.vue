<template>
  <div id="app">
    <Calculator />
  </div>
</template>

<script>
import Calculator from "./components/Calculator.vue";

export default {
  name: "App",
  components: {
    Calculator,
  },
};
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
}
#app {
  font-family: Arial, sans-serif;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.call{
  border: 1px solid red;
  width: 100%;
}
</style>
