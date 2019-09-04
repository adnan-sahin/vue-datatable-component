# vue datatable component
The v-data-table component is used for displaying json data as a table.

**Component Features**:
- MultiSorting 
- Draggable column 
- Searching 
- Column Visibility 

 **Usage:**
 
 **Component props:**
 **items**: It is used for passing json data as property.
 **headers**: It is used for passing table headers as property.
 **multisortable**: Specifies whether the component is multisortable or not.

**headers property details:**
 - **text**: It is used to display header text.
 - **value**: It is name of key in json data.
 - **visible**: It is used to show column or not.Default visible value is false.
```html
<template>
  <div id="app">
    <v-data-table :items="flights" :headers="headers" />
  </div>
</template>
```
```javascript
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
        { text: "Airline", value: "airline", visible: true },
        { text: "Flight No", value: "flightNo", visible: true },
        { text: "Origin", value: "origin", visible: true },
        { text: "Schedule Time", value: "scheduleTime", visible: true },
        { text: "Estimation Time", value: "estimationTime", visible: true },
        { text: "Note", value: "note", visible: true }
      ]
    };
  }
};
```
**Demo:** https://vue-datatable.netlify.com/

<a href="https://ibb.co/SVjDKPx"><img src="https://i.ibb.co/ZcjRhVJ/datatable1.png" alt="datatable1" border="0"></a><br />

**Column Visibility**

<a href="https://ibb.co/4Z4StfX"><img src="https://i.ibb.co/rG5p0tz/datatable2.png" alt="datatable2" border="0"></a><br />

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run start
```

### Compiles and minifies for production
```
npm run build
```


### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
