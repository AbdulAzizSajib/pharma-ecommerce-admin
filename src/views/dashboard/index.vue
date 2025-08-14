<script setup>
import { Icon } from "@iconify/vue";
import MainLayout from "@/components/layouts/mainLayout.vue";
</script>

<template>
  <MainLayout>
    <!-- Dashboard Section -->
    <div class="flex justify-start items-center gap-5">
      <!-- Today Stats -->
      <div class="bg-white p-6 rounded-lg shadow-xl border w-full space-y-8">
        <h2 class="text-4xl font-bold text-gray-800">Today</h2>
        <p class="text-xl text-gray-600 mt-2">
          Total Receive Amount:
          <span class="font-bold text-xl text-green-500">1000 taka</span>
        </p>
      </div>

      <!-- Monthly Stats -->
      <div class="bg-white p-6 rounded-lg shadow-xl border w-full space-y-8">
        <h2 class="text-4xl font-semibold text-gray-800">Monthly</h2>
        <p class="text-xl text-gray-600 mt-2">
          Total Receive Amount:
          <span class="font-bold text-blue-500">35000 taka</span>
        </p>
      </div>
    </div>

    <!-- Shortcut Links Section (Table Style) -->
    <div class="mt-8 flex items-center gap-5">
      <!-- Pending Orders Card -->
      <div class="p-8 rounded-lg shadow-lg mb-6 bg-slate-50 border-2">
        <div class="flex items-center justify-between mb-4">
          <div
            class="text-xl w-64 h-52 font-semibold text-black flex justify-center items-start flex-col space-y-5"
          >
            <Icon
              class="size-16 bg-red-600 text-white p-1 rounded"
              icon="ic:baseline-pending-actions"
            />
            <h2 class="text-xl font-bold">
              Pending Order List
              <span class="text-red-500 text-xl ml-2"> (100)</span>
            </h2>
          </div>
        </div>
      </div>

      <!-- All Orders Card -->
      <div class="p-8 rounded-lg shadow-lg mb-6 bg-slate-50 border-2">
        <div class="flex items-center justify-between mb-4">
          <div
            class="text-xl w-64 h-52 font-semibold text-black flex justify-center items-start flex-col space-y-5"
          >
            <Icon
              class="size-16 bg-red-600 text-white p-1 rounded"
              icon="ic:outline-select-all"
            />
            <h2 class="text-xl font-bold">
              Pending Order List
              <span class="text-red-500 text-xl ml-2"> (100)</span>
            </h2>
          </div>
        </div>
      </div>
    </div>
  </MainLayout>
</template>
<style scoped></style>
