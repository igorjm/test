<template>
  <div class="container mx-auto px-4 py-8">
    <DataTable :rows="filteredRows" />
  </div>
</template>

<script>
import Filter from "@/components/Filter.vue";
import DataTable from "@/components/DataTable.vue";
import data from "@/assets/data/deals/deals.json";

export default {
  components: {
    Filter,
    DataTable,
  },
  data() {
    return {
      searchQuery: "",
      rows: data.data,
    };
  },
  computed: {
    filteredRows() {
      if (!this.searchQuery) {
        return this.rows;
      } else {
        const query = this.searchQuery.toLowerCase();
        return this.rows.filter((row) => {
          return (
            row.DealName.toLowerCase().includes(query) ||
            row.Industry.toLowerCase().includes(query) ||
            row.Status.toLowerCase().includes(query)
          );
        });
      }
    },
  },
};
</script>