<template>
  <table id="custom-table">
    <thead>
      <tr>
        <th v-for="head in columns" :key="head">{{ head.displayText }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="d in filteredData" :key="d.id">
        <td v-for="head in columns" :key="head">{{d[head.key]}}</td>
      </tr>
    </tbody>
  </table>
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
import { defineComponent, ref, computed } from "vue";

export default defineComponent({
  props: ["columns", "dataset"],
  setup(props) {
    let data = ref(computed(() => props.dataset))
    let filteredData = ref([]);
    let currentPage = ref(1);
    let currentEntry = ref(10);
    let pages = ref(0);
    let showEnteries = ref([10, 25, 50, 100]);

    let pagination = () => {
      let trimStart = (currentPage.value - 1) * currentEntry.value;
      let trimEnd = trimStart + currentEntry.value;
      filteredData.value = data.value.slice(trimStart, trimEnd);
      pages.value = Math.ceil(data.value.length / currentEntry.value);
    };
    
    pagination();

    let changePage = (page) => {
      if (page === 'first') {
        currentPage.value = 1;
      } else if(page === 'previous') {
        currentPage.value = currentPage.value === 1 ? currentPage.value : currentPage.value - 1; 
      } else if (page === 'next') {
        currentPage.value = currentPage.value === pages.value ? currentPage.value : currentPage.value + 1;
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
      filteredData,
      changePage,
      showEnteries,
      currentPage,
      updatePages,
      currentEntry,
      data,
      pages
     }
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
