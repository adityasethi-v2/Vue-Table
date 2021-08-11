<template>
  <div v-if="data.length > 0">
    <TableData :columns="columns" :dataset="data"></TableData>
  </div>
</template>

<script>
import { ref } from "vue";
import TableData from "./components/TableData.vue";
import axios from "axios";

export default {
  components: {
    TableData,
  },
  setup() {
    let columns = ref([
      {
        displayText: "ID",
        key: "id",
      },
      {
        displayText: "First Name",
        key: "first_name",
      },
      {
        displayText: "Last Name",
        key: "last_name",
      },
      {
        displayText: "Email",
        key: "email",
      },
      {
        displayText: "Gender",
        key: "gender",
      },
      {
        displayText: "Asset",
        key: "asset",
      },
    ]);
    let data = ref([]);
    
    axios.get("data.json").then((res) => {
      data.value = res.data;
    });

    return {
      columns,
      data,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.btn {
  background-color: #fff; /* Green */
  border: none;
  color: black;
  padding: 8px 8px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

.selected {
  background-color: blue;
}

.pull-right {
  float: right;
}

.pull-left {
  float: left;
}

.padding {
  padding: 8px 8px;
}

.select-btn {
  cursor: pointer;
}
</style>
