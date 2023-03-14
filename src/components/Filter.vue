<template>
    <div
      class="p-3 max-w rounded-lg shadow-md dark:bg-gray-800 dark:border-gray-700"
    >
      <h5 class="mb-2 text-2xl font-bold dark:text-white">Filters</h5>
      <input
        type="search"
        id="search"
        v-model="search"
        class="block p-4 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        placeholder="Search Deals"
        required
      />
      <h5 class="my-2 text-lg font-bold dark:text-white">Sort by Column</h5>
      <select
        id="sort"
        v-model="sortBy"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-4 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
      >
        <option
          v-for="header in headers"
          :key="header.value"
          :value="header.value"
        >
          {{ header.text }}
        </option>
      </select>
      <div class="flex">
        <div
          class="flex items-center mt-4 mr-2"
          v-for="sort in sorts"
          :key="sort.value"
        >
          <input
            :id="`radio-${sort.value}`"
            type="radio"
            :value="sort.value"
            v-model="sortOrder"
            name="sort-radio"
            class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
          />
          <label
            :id="`radio-${sort.value}`"
            class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300"
            >{{ sort.text }}</label
          >
        </div>
      </div>
      <h5 class="my-2 text-lg font-bold dark:text-white">Columns</h5>
      <div
        class="flex items-center ml-1 my-2"
        v-for="header in headers"
        v-show="!header.hideFilter"
        :key="header.value"
      >
        <input
          :id="`checkbox-${header.value}`"
          type="checkbox"
          v-model="header.active"
          class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600 cursor-pointer"
        />
        <label
          :for="`checkbox-${header.value}`"
          class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300 cursor-pointer"
          >{{ header.text }}
        </label>
      </div>
      <div
        class="flex place-content-center p-3 space-x-2 rounded-b border-t border-gray-200 dark:border-gray-600"
      >
        <button
          @click="
            () => {
              $emit('cleanFilter');
              this.search = '';
              this.sortBy = 'dealName';
              this.sortOrder = 'asc';
            }
          "
          type="button"
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
        >
          Clean Filter
        </button>
      </div>
    </div>
  </template>
  
  
  <script>
  export default {
    name: 'Filter',
    data() {
      return {
        search: '',
        sortBy: 'dealName',
        sortOrder: 'asc',
        sorts: [
          { text: 'Ascending', value: 'asc' },
          { text: 'Descending', value: 'desc' },
        ],
        headers: [
          { text: 'Deal Name', value: 'dealName', active: true, hideFilter: false },
          { text: 'Stage', value: 'stage', active: true, hideFilter: false },
          { text: 'Closing Date', value: 'closingDate', active: true, hideFilter: false },
          { text: 'Amount', value: 'amount', active: true, hideFilter: false },
          { text: 'Contact', value: 'contact', active: true, hideFilter: false },
          { text: 'Owner', value: 'owner', active: true, hideFilter: false },
          { text: 'Created At', value: 'createdAt', active: true, hideFilter: true },
          { text: 'Updated At', value: 'updatedAt', active: true, hideFilter: true },
        ],
      };
    },
  };
</script>
  
  
<style scoped>
  .dark #search::-webkit-input-placeholder {
    color: #cbd5e0;
  }

  .dark #search:-moz-placeholder {
    color: #cbd5e0;
  }

  .dark #search::-moz-placeholder {
    color: #cbd5e0;
  }

  .dark #search:-ms-input-placeholder {
    color: #cbd5e0;
  }
</style>
