<template>
  <table id="custom-table">
    <thead>
      <tr>
        <th v-for="head in columns" :key="head">{{ head.displayText }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="data in dataset" :key="data.id">
        <td v-for="head in columns" :key="head">{{data[head.key]}}</td>
      </tr>
    </tbody>
  </table>
</template>
<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  props: ["columns", "dataset"],
  setup() {
  }
});
</script>
<style scoped>
#custom-table {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#custom-table td,
#custom-table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#custom-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

#custom-table tr:hover {
  background-color: #ddd;
}

#custom-table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
}
</style>
