<template>
  <v-app>
    <v-content>
      <FilterTable/>
    </v-content>
  </v-app>
</template>

<script>
import FilterTable from "./components/FilterTable.vue";

export default {
  name: "App",
  components: {
    FilterTable
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
  margin-top: 60px;
}
</style>