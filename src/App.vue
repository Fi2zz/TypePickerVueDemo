<template>
  <div id="app">
    <TypePicker></TypePicker>
  </div>
</template>

<script>
import TypePicker from "./TypePicker";
export default {
  name: "app",
  components: {
    TypePicker
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  align-items: center;
  margin: 100px auto 0;
  width: 640px;
}
</style>
