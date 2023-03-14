<template>
    <div class="fixed z-10 inset-0 overflow-y-auto">
      <div class="flex items-center justify-center min-h-screen">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75"></div>
        <div class="bg-white rounded-lg overflow-hidden">
          <div class="px-6 py-4">
            <h2 class="text-lg font-bold mb-2">{{ selectedRow.DealName }}</h2>
            <p class="text-gray-700 text-base">{{ selectedRow.Description }}</p>
          </div>
          <div class="px-6 py-4 bg-gray-100">
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
              @click="$emit('close-modal')">Close</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
    export default {
        props: {
        selectedRow: {
            type: Object,
            required: true,
        },
        },
    };
  </script>