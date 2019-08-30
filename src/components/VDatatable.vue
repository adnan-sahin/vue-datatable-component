<template>
  <div class="container">
    <table class="table table-striped">
      <thead class="thead-dark">
        <th v-for="(header,index) in headers" :key="index">{{header}}</th>
      </thead>
      <tbody>
        <tr v-for="(item,index) in items" :key="index">
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
  }
};
</script>
<style scoped>
</style>