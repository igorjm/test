<template>
  <div>
    <data-table :data="tableData" :columns="tableColumns" />
  </div>
</template>

<script>
  import DataTable from "./components/DataTable.vue";
  import tableData from "./assets/data/deals/deals.json";

  export default {
    components: {
      DataTable,
    },
    data() {
      return {
        tableData,
        tableColumns: ["Name", "Age", "Email"],
      };
    },
  };
</script>