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
            <th class="py-3 px-6 text-left font-semibold">Order Code</th>
            <th class="py-3 px-6 text-left font-semibold">Total</th>
            <th class="py-3 px-6 text-left font-semibold">Due</th>
            <th class="py-3 px-6 text-left font-semibold">Created At</th>
            <th class="py-3 px-6 text-left font-semibold">Country</th>
            <th class="py-3 px-6 text-left font-semibold">City</th>
          </tr>
        </thead>
        <tbody>
          <tr class="border-b hover:bg-gray-50" v-for="item in pendingOrder">
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
                Verify
              </button>
            </td>
            <td class="py-3 px-6 text-gray-800">{{ item?.sale_code }}</td>
            <td class="py-3 px-6 text-gray-800">{{ item?.total }} Taka</td>
            <td class="py-3 px-6 text-gray-800">{{ item?.amount_due }} Taka</td>
            <td class="py-3 px-6 text-gray-800">
              {{ formatDate(item?.created_at) }}
            </td>
            <td class="py-3 px-6 text-gray-800">BD</td>
            <td class="py-3 px-6 text-gray-800">Dhaka</td>
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
const pendingOrder = ref();
const getPendingOrder = async () => {
  loading.value = true;
  try {
    const res = await axios.get(
      `${apiBase}/admin/all-pending-order-list-paginated?page=1`,
      getTokenConfig()
    );
    loading.value = false;
    if (res.data) {
      pendingOrder.value = res?.data?.data;
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
  await getPendingOrder();
});
</script>

<style lang="scss" scoped></style>
