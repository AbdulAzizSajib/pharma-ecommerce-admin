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
          <tr class="border-b hover:bg-gray-50" v-for="item in AllOrder">
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
            <td class="py-3 px-6 text-gray-800">
              {{ formatDate(item?.sale_date) }}
            </td>
            <td class="py-3 px-6 text-gray-800">{{ item?.sale_code }}</td>

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

import { apiBase } from "@/config";
import { getTokenConfig } from "@/util/tokenConfig";
import axios from "axios";
import { onMounted, ref } from "vue";
const loading = ref(false);
const AllOrder = ref();
const getAllOrder = async () => {
  loading.value = true;
  try {
    const res = await axios.get(
      `${apiBase}/admin/all-order-list-paginated?page=1`,
      getTokenConfig()
    );
    loading.value = false;
    if (res.data) {
      AllOrder.value = res?.data?.data;
    }
  } catch (error) {
    console.log(error.message);
  }
};

const formatDate = (dateString) => {
  if (!dateString) return "";
  const date = new Date(dateString);
  return date.toISOString().split("T")[0]; // Formats as 'YYYY-MM-DD'
};

onMounted(async () => {
  await getAllOrder();
});
</script>

<style lang="scss" scoped></style>
