<template>
  <div class="container">
    <div class="toolbar">
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
      <div class="search--container">
        <input type="text" placeholder="Search" v-model="search" />
      </div>
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
            <span>{{ header.text }}</span>
            <span class="inline-block">
              <i :class="sortIconClass('asc',header.value)" class="fas fa-sort-up arrow"></i>
              <i :class="sortIconClass('desc',header.value)" class="fas fa-sort-down arrow"></i>
            </span>
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
    items: { required: true, type: Array },
    multisortable: { required: false, type: Boolean, default: true }
  },
  components: { draggable },
  data() {
    return {
      currentSortName: this.headers[0].value,
      currentSortDirection: "asc",
      search: "",
      toggleVisibility: false,
      headerItems: this.headers,
      dataItems: this.items,
      sortedColumns: [{ name: this.headers[0].value, direction: "asc" }]
    };
  },
  computed: {
    sortedItems() {
      const items = this.dataItems;
      return items.sort((a, b) => {
        let modifier = 1;
        if (this.currentSortDirection === "desc") modifier = -1;
        if (a[this.currentSortName] < b[this.currentSortName])
          return -1 * modifier;
        if (a[this.currentSortName] > b[this.currentSortName])
          return 1 * modifier;
        return 0;
      });
    }
  },
  methods: {
    sort(sortName) {
      const column = this.sortedColumns.find(s => s.name == sortName);
      if (column) {
        this.currentSortDirection =
          this.currentSortDirection === "asc" ? "desc" : "asc";
        column.direction = column.direction === "asc" ? "desc" : "asc";
      } else {
        this.currentSortDirection = "asc";
      }
      if (this.multisortable) {
        if (!column) {
          this.sortedColumns.push({
            name: sortName,
            direction: this.currentSortDirection
          });
        }
      } else {
        this.sortedColumns = [
          {
            name: sortName,
            direction: this.currentSortDirection
          }
        ];
      }
      this.currentSortName = sortName;
    },
    sortIconClass(sortDirection, sortName) {
      return this.sortedColumns.find(
        s => s.name == sortName && s.direction == sortDirection
      )
        ? "active"
        : "";
    }
  },
  watch: {
    search: function() {
      const result = [];
      const values = Object.values(this.items);
      for (let item of values) {
        let row = Object.values(item);
        for (let col of row) {
          if (
            col.toLowerCase().indexOf(this.search.toLowerCase()) !== -1 &&
            result.filter(a => a == item).length == 0
          ) {
            result.push(item);
            break;
          }
        }
      }
      this.dataItems = result;
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
  background-color: #1871b9;
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
.toolbar {
  position: relative;
  width: 100%;
  text-align: left;
}
.column--visibility {
  display: inline-block;
  margin: 10px 0;
  font-size: 0.9rem;
  text-align: left;
  width: 50%;
}
.search--container {
  display: inline-block;
  margin: 10px 0;
  font-size: 0.9rem;
  text-align: right;
  width: 50%;
}
.search--container input {
  padding: 7px 10px;
  border-radius: 5px;
  outline: none;
  border: 1px solid #7a7d80;
}
.column--visibility label.toggle {
  color: white;
  padding: 10px;
  background: #1871b9;
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
  top: 35px;
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
  background-color: #2d90fa !important;
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
.inline-block {
  display: inline-block;
}
.arrow {
  display: block;
  line-height: 0.1;
  font-size: 1rem;
  margin-left: 5px;
  color: rgb(126, 126, 126);
}
.arrow.active {
  color: #fff;
}
@media screen and (max-width: 720px) {
  .column--visibility {
    width: 100%;
    text-align: center;
  }
  .search--container {
    width: 100%;
    text-align: center;
  }
  .headers {
    position: static;
  }
}
</style>