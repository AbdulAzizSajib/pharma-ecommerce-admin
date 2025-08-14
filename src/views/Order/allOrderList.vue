<template>
  <MainLayout>
    <div class="p-6">
      <!-- Pending List Table -->
      <table
        class="min-w-full bg-white border border-gray-200 rounded-lg shadow-md"
      >
        <thead>
          <tr class="bg-gray-100 text-gray-600">
            <th class="py-3 px-6 text-left font-semibold">Action</th>
            <th class="py-3 px-6 text-left font-semibold">Date</th>
            <th class="py-3 px-6 text-left font-semibold">Order Code</th>

            <th class="py-3 px-6 text-left font-semibold">Country</th>
            <th class="py-3 px-6 text-left font-semibold">City</th>
            <th class="py-3 px-6 text-left font-semibold">Status</th>
          </tr>
        </thead>
        <tbody>
          <tr class="border-b hover:bg-gray-50">
            <td class="py-3 px-6 flex gap-2">
              <router-link :to="{ name: 'view' }">
                <button
                  class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition duration-300"
                >
                  View
                </button>
              </router-link>
              <button
                class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition duration-300"
              >
                Receive
              </button>
              <button
                class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-green-700 transition duration-300"
              >
                Return
              </button>
            </td>
            <td class="py-3 px-6 text-gray-800">01/02/2025</td>
            <td class="py-3 px-6 text-gray-800">01FG4532</td>

            <td class="py-3 px-6 text-gray-800">BD</td>
            <td class="py-3 px-6 text-gray-800">Dhaka</td>
            <td class="py-3 px-6 text-yellow-500">Pending</td>
          </tr>
        </tbody>
      </table>
    </div>
  </MainLayout>
</template>

<script setup>
import MainLayout from "@/components/layouts/mainLayout.vue";
</script>

<style lang="scss" scoped></style>
