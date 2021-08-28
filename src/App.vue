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
  // font-family: "Raleway", sans-serif;
}
.container {
  margin: 50px auto;
}
body {
  padding: 0px;
  margin: 0px;
}
</style>
