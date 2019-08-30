<template>
  <div id="app">
    <v-data-table :items="flights" :headers="headers" />
  </div>
</template>

<script>
import VDatatable from "./components/VDatatable.vue";
import flights from "@/data/flights.json";
export default {
  name: "app",
  components: {
    "v-data-table": VDatatable
  },
  data() {
    return {
      flights: flights,
      headers: [
        "Date",
        "Airline",
        "Flight No",
        "Origin",
        "Schedule Time",
        "Estimation Time",
        "Note"
      ]
    };
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
