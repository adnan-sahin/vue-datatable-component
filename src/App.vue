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
        { text: "Date", value: "date", visible: true },
        { text: "Airline", value: "airline", visible: false },
        { text: "Flight No", value: "flightNo", visible: true },
        { text: "Origin", value: "origin", visible: true },
        { text: "Schedule Time", value: "scheduleTime", visible: true },
        { text: "Estimation Time", value: "estimationTime", visible: true },
        { text: "Note", value: "note", visible: true }
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
