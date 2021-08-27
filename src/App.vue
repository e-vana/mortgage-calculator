<template>
  <div id="app">
    <div class="container">
      <Calculator />
    </div>
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

<style lang="scss">
* {
  font-family: "Montserrat", sans-serif;
  font-family: "Raleway", sans-serif;
}
</style>
