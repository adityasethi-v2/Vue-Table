<template>
  <div>
    Select
    <select v-model="currentEntry" @change="updatePages">
      <option v-for="(entry, i) in showEnteries" :key="i" :value="entry">{{ entry }}</option>
    </select>
    Enteries
  </div>
  <TableData :columns="columns" :dataset="filteredData"></TableData>
  <div>
    <button
      class="btn"
      :class="{ selected: currentPage === page }"
      v-for="(page, i) in pages"
      :key="i"
      @click="updateData(page)"
    >
      {{ page }}
    </button>
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
    let filteredData = ref([]);
    let currentPage = ref(1);
    let currentEntry = ref(5);
    let pages = ref(0);
    let showEnteries = ref([5, 10, 15, 25, 50]);

    axios.get("data.json").then((res) => {
      data.value = res.data;
      pagination();
    });

    let pagination = () => {
      let trimStart = (currentPage.value - 1) * currentEntry.value;
      let trimEnd = trimStart + currentEntry.value;
      filteredData.value = data.value.slice(trimStart, trimEnd);
      pages.value = Math.ceil(data.value.length / currentEntry.value);
    };

    let updateData = (page) => {
      currentPage.value = page;
      pagination();
    };

    let updatePages = () => {
      currentPage.value = 1;
      pagination();
    }

    return {
      columns,
      filteredData,
      showEnteries,
      currentEntry,
      pagination,
      pages,
      updateData,
      currentPage,
      updatePages
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
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 8px 8px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

.selected {
  background-color: orange;
}
</style>
