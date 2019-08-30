<template>
  <div class="container">
    <table class="table table-striped">
      <thead class="thead-dark">
        <th v-for="(header,index) in headers" :key="index" @click="sort(header.value)">
          {{header.text}}
          <template v-if="currentSortName==header.value">
            <i :class="sortIconClass"></i>
          </template>
        </th>
      </thead>
      <tbody>
        <tr v-for="(item,index) in sortedItems" :key="index">
          <td v-for="(value,index) in item" :key="index">{{value}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  props: {
    headers: { required: true, type: Array },
    items: { required: true, type: Array }
  },
  data() {
    return {
      currentSortName: this.headers[0].value,
      currentSortDirection: "asc"
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
</style>