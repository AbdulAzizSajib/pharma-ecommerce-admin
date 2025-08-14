<script setup>
import MainLayout from "@/components/layouts/mainLayout.vue";
</script>

<template>
  <MainLayout>
    <!-- Dashboard Section -->
    <div class="flex justify-start items-center gap-5">
      <!-- Today Stats -->
      <div class="bg-white p-6 rounded-lg shadow-lg border w-full">
        <h2 class="text-2xl font-semibold text-gray-800">Today</h2>
        <p class="text-xl text-gray-600 mt-2">
          Total Receive Amount:
          <span class="font-bold text-green-500">1000 taka</span>
        </p>
      </div>

      <!-- Monthly Stats -->
      <div class="bg-white p-6 rounded-lg shadow-lg border w-full">
        <h2 class="text-2xl font-semibold text-gray-800">Monthly</h2>
        <p class="text-xl text-gray-600 mt-2">
          Total Receive Amount:
          <span class="font-bold text-blue-500">35000 taka</span>
        </p>
      </div>
    </div>

    <!-- Shortcut Links Section (Table Style) -->
    <div class="mt-8">
      <!-- Pending Order List Table -->
      <table class="min-w-full text-left">
        <thead>
          <tr class="border-b">
            <th class="py-2 px-4 text-xl font-semibold text-gray-800">
              Pending Order List <span class="text-red-500">(100)</span>
            </th>
            <th class="py-2 px-4 text-right">
              <button
                class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition duration-300"
              >
                View All
              </button>
            </th>
          </tr>
        </thead>
        <tbody>
          <!-- Add table rows here if needed -->
        </tbody>
      </table>

      <!-- All Order List Table -->
      <table class="min-w-full text-left mt-4">
        <thead>
          <tr class="border-b">
            <th class="py-2 px-4 text-xl font-semibold text-gray-800">
              All Order List
            </th>
            <th class="py-2 px-4 text-right">
              <button
                class="px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition duration-300"
              >
                View All
              </button>
            </th>
          </tr>
        </thead>
        <tbody>
          <!-- Add table rows here if needed -->
        </tbody>
      </table>
    </div>
  </MainLayout>
</template>
