<template>
  <div class="container mx-auto px-4 py-8">
    <table class="table-auto w-full border shadow-md">
      <thead class="text-xs text-gray-700 uppercase">
        <tr class="bg-gray-100 text-gray-600 uppercase">
          <th
            v-for="(column, index) in columns"
            :key="column.key"
            @click="sortBy(column.key)"
            :class="{
              'cursor-pointer': column.sortable,
              'pr-4': index < columns.length - 1,
              'pl-4': index === 0
            }"
            class="py-2"
          >
            {{ column.label }}
            <span v-if="sortColumn === column.key" class="ml-1">
              {{ sortDirection === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="row in displayedRows" :key="row.Id">
          <td v-for="column in columns" :key="column.key" class="p-2">
            <template v-if="column.key !== 'Actions'">
              {{ row[column.key] }}
            </template>
            <template v-else>
              <button @click="showModal(row)">View Details</button>
            </template>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="flex justify-end mt-4">
      <button
        class="bg-gray-200 hover:bg-gray-300 py-2 px-4 rounded-l"
        :disabled="currentPage === 1"
        @click="currentPage--"
      >
        Previous
      </button>
      <button
        class="bg-gray-200 hover:bg-gray-300 py-2 px-4 rounded-r"
        :disabled="currentPage === pageCount"
        @click="currentPage++"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rows: [],
      columns: [
        { key: "Id", label: "ID", sortable: true },
        { key: "DealName", label: "Deal Name", sortable: true },
        { key: "IndustryId", label: "Industry", sortable: true },
        { key: "StatusId", label: "Status", sortable: true },
        { key: "Created", label: "Created", sortable: true },
        {
          key: 'Actions',
          label: 'Actions',
          sortable: false,
          class: "text-center",
        },
      ],
      sortColumn: "Id",
      sortDirection: "asc",
      currentPage: 1,
      pageSize: 10,
      searchText: '',
    };
  },
  computed: {
    sortedRows() {
      return this.rows.slice().sort((a, b) => {
        const sortOrder = this.sortDirection === "asc" ? 1 : -1;
        const aVal = a[this.sortColumn];
        const bVal = b[this.sortColumn];

        if (typeof aVal === "number" && typeof bVal === "number") {
          return sortOrder * (aVal - bVal);
        } else {
          return sortOrder * aVal.localeCompare(bVal);
        }
      });
    },
    pageCount() {
      return Math.ceil(this.sortedRows.length / this.pageSize);
    },
    displayedRows() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = start + this.pageSize;
      const rows = this.sortedRows.slice(start, end);

      // Add button to each row in the Action column
      rows.forEach((row) => {
        row.Action = `<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          @click="showModal(${row.Id})">View Details</button>`;
      });

      return rows;
    },
    filteredDeals() {
      return this.displayedRows.filter(deal => {
        return Object.values(deal).some(value => {
          if (typeof value === 'string') {
            return value.toLowerCase().includes(this.searchText.toLowerCase());
          }
          return false;
        });
      });
    },
  },
  methods: {
    async loadData() {
      const response = await fetch("/src/assets/data/deals/deals.json");
      const json = await response.json();
      this.rows = json.data;
    },
    sortBy(column) {
      if (this.sortColumn === column) {
        this.sortDirection = this.sortDirection === "asc" ? "desc" : "asc";
      } else {
        this.sortColumn = column;
        this.sortDirection = "asc";
      }
    },
    showModal(row) {
      this.selectedRow = row;
      this.showModalWindow = true;
    }
  },
  async mounted() {
    await this.loadData();
  },
};
</script>
