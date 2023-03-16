<template>
  <div class="my-4">
    <input
      v-model="filterText"
      type="text"
      class="px-4 py-2 border border-gray-400 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
      placeholder="Search by deal name..."
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      filterText: "",
    };
  },
  watch: {
    filterText() {
      this.$emit("filter-changed", this.filterText);
    },
  },
};
</script>

<style>
input:focus {
  border-color: #667eea;
  outline: none;
  box-shadow: none;
}
</style>
