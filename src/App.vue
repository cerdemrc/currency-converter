<template>
  <div id="app">
    <Currency />
  </div>
</template>

<script>
import Currency from "./components/Currency";
export default {
  components: {
    Currency,
  },
};
</script>

<style>
body {
  background: #2c3e50;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}
</style>
