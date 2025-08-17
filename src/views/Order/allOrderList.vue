<template>
  <MainLayout>
    <div class="">
      <!-- Pending List Table -->
      <table
        class="min-w-full bg-white border border-gray-200 rounded-lg shadow-md"
      >
        <thead>
          <tr class="bg-gray-100 text-gray-600">
            <th class="py-3 px-6 text-left font-semibold">SL</th>
            <th class="py-3 px-6 w-32 text-center font-semibold">Action</th>
            <th class="py-3 px-6 text-left font-semibold">Date</th>
            <th class="py-3 px-6 text-left font-semibold">Order Code</th>

            <th class="py-3 px-6 text-left font-semibold">Country</th>
            <th class="py-3 px-6 text-left font-semibold">City</th>
            <th class="py-3 px-6 text-left font-semibold">Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="loading">
            <td><h2 class="text-2xl text-left px-2">loading...</h2></td>
          </tr>
          <tr
            v-else
            class="border-b hover:bg-gray-50"
            v-for="(item, index) in AllOrder"
          >
            <td class="py-3 px-6 text-gray-800">{{ index + 1 }}</td>
            <td class="py-3 px-6 flex justify-center gap-2">
              <router-link :to="{ name: 'view', params: { id: item?.id } }">
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
              {{ formatDate(item?.sale_date || "-") }}
            </td>
            <td class="py-3 px-6 text-gray-800">
              {{ item?.sale_code || "-" }}
            </td>

            <td class="py-3 px-6 text-gray-800">
              {{ item?.billing_address?.country?.name || "-" }}
            </td>
            <td class="py-3 px-6 text-gray-800">
              {{ item?.billing_address?.city?.name || "-" }}
            </td>
            <td class="py-3 px-6 text-yellow-500">Pending</td>
          </tr>
        </tbody>
      </table>
    </div>

    <a-pagination
      v-if="!loading"
      class="mt-5"
      v-model:current="currentPage"
      :total="backupData?.total"
      :show-size-changer="false"
      v-model:pageSize="pageSize"
      :show-total="(total) => `Total ${total} items`"
      @change="pagination"
    />
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
const backupData = ref();
const pageSize = ref();
const currentPage = ref(1);
const pagination = async (page) => {
  currentPage.value = page;
  await getAllOrder();
};

const getAllOrder = async () => {
  loading.value = true;
  try {
    const res = await axios.get(
      `${apiBase}/admin/all-order-list-paginated?page=${currentPage.value}`,
      getTokenConfig()
    );
    loading.value = false;
    if (res.data) {
      AllOrder.value = res?.data?.data;
      backupData.value = res?.data;
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
