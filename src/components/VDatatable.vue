<template>
  <div class="container">
    <div class="column--visibility">
      <label class="toggle" :class="toggleVisibility?'bg--primary':''">
        <input class="chc--toggle" type="checkbox" v-model="toggleVisibility" />
        Column Visibility
        <ul class="headers">
          <li v-for="(header,index) in headerItems" :key="index">
            <label :class="header.visible?'':'bg--disabled'">
              <input class="chc--header" type="checkbox" v-model="header.visible" />
              {{header.text}}
            </label>
          </li>
        </ul>
      </label>
    </div>

    <table class="table table-striped">
      <thead class="thead-dark">
        <draggable v-model="headerItems" tag="tr">
          <th
            v-for="(header,index) in headerItems"
            :key="index"
            @click="sort(header.value)"
            v-show="header.visible"
            scope="col"
          >
            {{ header.text }}
            <template v-if="currentSortName==header.value">
              <i :class="sortIconClass"></i>
            </template>
          </th>
        </draggable>
      </thead>
      <tbody>
        <tr v-for="(item,index) in sortedItems" :key="index">
          <td
            v-for="(header,index) in headerItems"
            :key="index"
            v-show="headerItems.filter(h=>h.value==header.value)[0]['visible']"
          >{{ item[header.value] }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import draggable from "vuedraggable";
export default {
  props: {
    headers: { required: true, type: Array },
    items: { required: true, type: Array }
  },
  components: { draggable },
  data() {
    return {
      currentSortName: this.headers[0].value,
      currentSortDirection: "asc",
      toggleVisibility: false,
      headerItems: this.headers
    };
  },
  computed: {
    sortedItems() {
      return this.items.slice().sort((a, b) => {
        let modifierDirection = 1;
        if (this.currentSortDirection === "desc") modifierDirection = -1;
        if (a[this.currentSortName] < b[this.currentSortName])
          return -1 * modifierDirection;
        if (a[this.currentSortName] > b[this.currentSortName])
          return 1 * modifierDirection;
        return 0;
      });
    },
    sortIconClass() {
      return this.currentSortDirection == "asc"
        ? "fas fa-sort-up"
        : "fas fa-sort-down";
    }
  },
  methods: {
    sort(sortName) {
      if (sortName === this.currentSortName) {
        this.currentSortDirection =
          this.currentSortDirection === "asc" ? "desc" : "asc";
      }
      this.currentSortName = sortName;
    }
  }
};
</script>
<style scoped>
.table {
  width: 100%;
  margin-bottom: 1rem;
  color: #212529;
  border-collapse: collapse;
  font-size: 0.8rem;
}
.table th,
.table td {
  padding: 0.75rem;
  vertical-align: top;
  border-spacing: 0;
  border-top: 1px solid #dee2e6;
}

.table thead th {
  vertical-align: bottom;
  border-bottom: 2px solid #dee2e6;
  cursor: pointer;
}
.thead-dark {
  background-color: #343a40;
  color: #fff;
}
.table-striped tbody tr:nth-of-type(odd) {
  background-color: rgba(0, 0, 0, 0.05);
}
.container {
  width: 90%;
  margin: 0 auto;
  max-width: 1400px;
}
.column--visibility {
  display: block;
  margin: 10px 0;
  position: relative;
  text-align: left;
  font-size: 0.9rem;
}
.column--visibility label.toggle {
  color: white;
  padding: 10px;
  background: #343a40;
  border: none;
  border-radius: 3px;
  outline: none;
  min-width: 170px;
  display: inline-block;
  box-sizing: border-box;
  text-align: center;
}
.column--visibility label.toggle:active {
  background: #5e5c5c;
}

.column--visibility ul {
  list-style: none;
  padding: 5px 0;
}
.column--visibility ul li {
  padding: 2px 4px;
  font-weight: bold;
  text-align: center;
}
.headers {
  display: none;
  background: white;
  border: 1px solid grey;
  border-radius: 3px;
  position: absolute;
  top: 25px;
  min-width: 170px;
  left: 0;
  z-index: 2;
  color: black;
  box-sizing: border-box;
}
.headers label {
  background: rgb(217, 219, 217);
  padding: 5px 0;
  min-width: 150px;
  box-sizing: border-box;
  border-radius: 3px;
  display: inline-block;
  cursor: pointer;
}
label.bg--disabled {
  background-color: #edf1f3 !important;
  color: #928f8f !important;
}
.bg--primary {
  background-color: #007bff !important;
}
.column--visibility .chc--toggle:checked ~ .headers {
  display: block;
}
.chc--toggle {
  display: none;
}
.chc--header {
  display: none;
}
</style>