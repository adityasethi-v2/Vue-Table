<template>
  <TableData :columns="columns" :dataset="filteredData"></TableData>
  <div class="page-change-section pull-left">
    <div class="pull-left" v-if="currentEntry !== data.length">
      <span class=" padding select-btn" @click="changePage('first')">&laquo;</span>
      <span class=" padding select-btn" @click="changePage('previous')">&#8249;</span>
      <span class=" padding text">Showing {{ currentPage }} of {{ pages }}</span>
      <span class=" padding select-btn" @click="changePage('next')">&#8250;</span>
      <span class=" padding select-btn" @click="changePage('last')">&raquo;</span>
    </div>
  </div>
  <div class="select-entry-section pull-right">
    <button
      class="btn"
      :class="{ selected: currentEntry === data.length }"
      @click="updatePages(data.length)"
    >
      All
    </button>
    <button
      class="btn"
      v-for="(entry, i) in showEnteries"
      :key="i"
      :class="{ selected: currentEntry === entry }"
      @click="updatePages(entry)"
    >
      {{ entry }}
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
    let currentEntry = ref(10);
    let pages = ref(0);
    let showEnteries = ref([10, 25, 50, 100]);

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

    let changePage = (page) => {
      if (page === 'first') {
        currentPage.value = 1;
      } else if(page === 'previous') {
        currentPage.value = currentPage.value === 1 ? currentPage.value : currentPage.value - 1; 
      } else if (page === 'next') {
        currentPage.value = currentPage.value === pages.value.length ? currentPage.value : currentPage.value + 1;
      } else if (page === 'last') {
        currentPage.value = pages.value;
      }
      pagination();
    };

    let updatePages = (entry) => {
      currentEntry.value = entry;
      currentPage.value = 1;
      pagination();
    };

    return {
      columns,
      filteredData,
      showEnteries,
      currentEntry,
      pagination,
      pages,
      changePage,
      currentPage,
      updatePages,
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