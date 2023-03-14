<template>
  <div>
    <input type="text" v-model="filter" placeholder="Filtrar dados">
    <table>
      <thead>
        <tr>
          <th v-for="(col, index) in columns" :key="index">
            {{ col }}
            <span @click="sortBy(index)">▲</span>
            <span @click="sortBy(index, true)">▼</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in filteredData" :key="index">
          <td v-for="(col, index) in row" :key="index">{{ col }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      required: true,
    },
    columns: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      filter: '',
      sortByIndex: null,
      sortAsc: true,
    };
  },
  computed: {
    filteredData() {
      if (!this.filter) {
        return this.data;
      }
      return this.data.filter((row) =>
        row.some((cell) => cell.includes(this.filter))
      );
    },
  },
  methods: {
    sortBy(index, reverse = false) {
      if (index === this.sortByIndex) {
        this.sortAsc = !this.sortAsc;
      } else {
        this.sortByIndex = index;
        this.sortAsc = true;
      }

      this.filteredData.sort((a, b) => {
        const aVal = a[index];
        const bVal = b[index];
        if (aVal === bVal) {
          return 0;
        }
        if (reverse) {
          return aVal < bVal ? 1 : -1;
        }
        return aVal > bVal ? 1 : -1;
      });
    },
  },
};
</script>
