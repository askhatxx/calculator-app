<template>
  <div id="app">
    <Calculator />
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'app',
  components: {
    Calculator
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: #f2f2f2;
  font-family: Arial, sans-serif;
}
#app {
  display: flex;
  justify-content: center;
  padding-top: 50px;
}
</style>
