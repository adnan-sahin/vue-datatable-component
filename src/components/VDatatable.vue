<template>
  <div class="container">
    <div class="column--visibility">
      <label class="toggle" :class="toggleVisibility?'bg--primary':''">
        <input class="chc--toggle" type="checkbox" v-model="toggleVisibility" />
        Column Visibility
        <ul class="headers">
          <li v-for="(header,index) in headerItems" :key="index">
            <label>
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
      return this.items.sort((a, b) => {
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
table thead th {
  cursor: pointer;
}
.column--visibility {
  text-align: left;
  display: block;
  margin: 10px 0;
  position: relative;
}
.column--visibility label.toggle {
  color: white;
  padding: 7px 10px;
  background: #444444;
  border: none;
  border-radius: 3px;
  outline: none;
  min-width: 170px;
  box-sizing: border-box;
}
.column--visibility label.toggle:active {
  background: #5e5c5c;
}

.column--visibility ul {
  list-style: none;
  padding: 5px 0;
  margin-top: 10px;
}
.column--visibility ul li {
  padding: 2px 4px;
  font-weight: bold;
}
.headers {
  display: none;
  background: white;
  border: 1px solid grey;
  border-radius: 3px;
  position: absolute;
  z-index: 2;
  color: black;
}
.column--visibility .chc--toggle:checked ~ .headers {
  display: block;
}
.chc--toggle {
  visibility: hidden;
}
.bg--primary {
  background-color: #007bff !important;
}
</style>