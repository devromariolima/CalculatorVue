<template>
  <div id="app">
    <h1>Calculadora</h1>
    <Calculator />
  </div>
</template>

<script>
import Calculator from './mainpage/CalculatorPage.vue';

export default {

  components: { Calculator }

}

</script>


<style>
* {
  font-family: 'Courier New', Courier, monospace;
}

body {
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
  background: linear-gradient(to bottom, #1e3c72, #e0e0e0);

}
</style>