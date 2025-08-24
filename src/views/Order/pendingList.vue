<template>
  <MainLayout>
    <div class="bg-gray-50 min-h-screen">
      <!-- Page Header -->
      <div
        class="bg-white rounded-xl shadow-sm border border-gray-200 p-6 mb-8"
      >
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-4">
            <div
              class="w-12 h-12 bg-orange-100 rounded-lg flex items-center justify-center"
            >
              <Icon icon="mdi:clock-outline" class="w-6 h-6 text-orange-600" />
            </div>
            <div>
              <h1 class="text-2xl font-bold text-gray-900">Pending Orders</h1>
              <p class="text-sm text-gray-500 mt-1">
                Manage and process pending order requests
              </p>
            </div>
          </div>
          <div class="flex items-center space-x-3">
            <span
              class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-orange-100 text-orange-800"
            >
              {{ pendingOrder?.length || 0 }} Pending
            </span>
          </div>
        </div>
      </div>

      <!-- Orders Table -->
      <div
        class="bg-white rounded-xl shadow-sm border border-gray-200 overflow-hidden"
      >
        <!-- Table Header -->
        <div class="px-6 py-4 border-b border-gray-200 bg-gray-50">
          <h2 class="text-lg font-semibold text-gray-900 flex items-center">
            <Icon
              icon="mdi:format-list-bulleted"
              class="w-5 h-5 mr-2 text-gray-600"
            />
            Orders Awaiting Processing
          </h2>
        </div>

        <!-- Loading State -->
        <div v-if="loading" class="p-12">
          <div class="flex flex-col items-center justify-center space-y-4">
            <Icon
              class="w-12 h-12 animate-spin text-orange-600"
              icon="mage:reload"
            />
            <h3 class="text-xl font-medium text-gray-900">
              Loading pending orders...
            </h3>
            <p class="text-gray-500">
              Please wait while we fetch the latest data
            </p>
          </div>
        </div>

        <!-- Table Content -->
        <div v-else class="overflow-x-auto">
          <table class="w-full">
            <thead class="bg-gray-50 border-b border-gray-200">
              <tr>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  #
                </th>

                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  User / Guest
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Order Code
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Total Amount
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Amount Due
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Order Date
                </th>
                <!-- <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Country
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  City
                </th> -->
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Actions
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200">
              <tr
                v-for="(item, index) in pendingOrder"
                :key="item.id"
                class="hover:bg-gray-50 transition-colors duration-200"
              >
                <!-- Serial Number -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <span
                      class="inline-flex items-center justify-center w-8 h-8 bg-blue-100 text-blue-800 text-sm font-medium rounded-full"
                    >
                      {{ (currentPage - 1) * (pageSize || 10) + index + 1 }}
                    </span>
                  </div>
                </td>

                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <div>
                      <div class="text-sm font-semibold text-gray-900">
                        {{ item?.customer_id === null ? "Guest " : "User " }}
                      </div>
                    </div>
                  </div>
                </td>
                <!-- Order Code -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <div>
                      <div class="text-sm font-medium text-gray-900">
                        {{ item?.sale_code }}
                      </div>
                      <div class="text-xs text-gray-500">
                        Order #{{ item?.id }}
                      </div>
                    </div>
                  </div>
                </td>

                <!-- Total Amount -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm font-bold text-gray-900">
                    ৳{{ item?.total?.toLocaleString() }}
                  </div>
                </td>

                <!-- Amount Due -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm font-medium text-gray-900">
                    ৳{{ item?.amount_due?.toLocaleString() }}
                  </div>
                </td>

                <!-- Created Date -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-gray-900">
                    {{ formatDate(item?.created_at) }}
                  </div>
                  <div class="text-xs text-gray-500">
                    {{ formatTime(item?.created_at) }}
                  </div>
                </td>

                <!-- Country -->
                <!-- <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <Icon icon="mdi:earth" class="w-4 h-4 text-gray-400 mr-2" />
                    <span class="text-sm text-gray-900">
                      {{ item?.billing_address?.country?.name || "N/A" }}
                    </span>
                  </div>
                </td> -->

                <!-- City -->
                <!-- <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <Icon icon="mdi:city" class="w-4 h-4 text-gray-400 mr-2" />
                    <span class="text-sm text-gray-900">
                      {{ item?.billing_address?.city?.name || "N/A" }}
                    </span>
                  </div>
                </td> -->
                <!-- Actions -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center space-x-2">
                    <router-link
                      :to="{ name: 'view', params: { id: item?.id } }"
                    >
                      <button
                        class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-lg text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-all duration-200"
                        title="View Order Details"
                      >
                        <Icon icon="mdi:eye" class="w-4 h-4" />
                      </button>
                    </router-link>
                    <router-link
                      :to="{ name: 'verify', params: { id: item?.id } }"
                    >
                      <button
                        class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-lg text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 transition-all duration-200"
                        title="Verify Order"
                      >
                        <Icon icon="mdi:check-circle" class="w-4 h-4 mr-1" />
                        <span class="hidden sm:inline">Verify</span>
                      </button>
                    </router-link>
                  </div>
                </td>
              </tr>

              <!-- Empty State -->
              <tr
                v-if="!loading && (!pendingOrder || pendingOrder.length === 0)"
              >
                <td colspan="8" class="px-6 py-12 text-center">
                  <div
                    class="flex flex-col items-center justify-center space-y-4"
                  >
                    <div
                      class="w-16 h-16 bg-gray-100 rounded-full flex items-center justify-center"
                    >
                      <Icon
                        icon="mdi:package-variant"
                        class="w-8 h-8 text-gray-400"
                      />
                    </div>
                    <div>
                      <h3 class="text-lg font-medium text-gray-900">
                        No pending orders
                      </h3>
                      <p class="text-gray-500 mt-1">
                        All orders have been processed or there are no new
                        orders.
                      </p>
                    </div>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- Table Footer -->
        <div
          v-if="!loading && pendingOrder && pendingOrder.length > 0"
          class="px-6 py-4 bg-gray-50 border-t border-gray-200"
        >
          <div class="flex items-center justify-between">
            <div class="text-sm text-gray-700">
              Showing {{ (currentPage - 1) * (pageSize || 10) + 1 }} to
              {{
                Math.min(currentPage * (pageSize || 10), backupData?.total || 0)
              }}
              of {{ backupData?.total || 0 }} orders
            </div>
            <a-pagination
              v-model:current="currentPage"
              :total="backupData?.total"
              :show-size-changer="false"
              v-model:pageSize="pageSize"
              :show-total="(total) => `Total ${total} orders`"
              @change="pagination"
              class="flex items-center"
            />
          </div>
        </div>
      </div>
    </div>
  </MainLayout>
</template>

<script setup>
import MainLayout from "@/components/layouts/mainLayout.vue";
import { apiBase } from "@/config";
import { getTokenConfig } from "@/util/tokenConfig";
import { Icon } from "@iconify/vue";
import axios from "axios";
import { onMounted, ref } from "vue";

const loading = ref(false);
const pendingOrder = ref();
const backupData = ref();
const pageSize = ref(10);
const currentPage = ref(1);

const pagination = async (page) => {
  currentPage.value = page;
  await getPendingOrder();
};

const getPendingOrder = async () => {
  loading.value = true;
  try {
    const res = await axios.get(
      `${apiBase}/admin/all-pending-order-list-paginated?page=${currentPage.value}`,
      getTokenConfig()
    );
    loading.value = false;
    if (res?.data) {
      pendingOrder.value = res?.data?.data;
    }
  } catch (error) {
    console.log(error.message);
  }
};

const formatDate = (dateString) => {
  if (!dateString) return "";
  const date = new Date(dateString);
  return date.toLocaleDateString("en-US", {
    year: "numeric",
    month: "short",
    day: "numeric",
  });
};

const formatTime = (dateString) => {
  if (!dateString) return "";
  const date = new Date(dateString);
  return date.toLocaleTimeString("en-US", {
    hour: "2-digit",
    minute: "2-digit",
  });
};

const handleCancelOrder = async (id) => {
  try {
    const res = await axios.post(`${apiBase}/sale/request-to-suspend/${id}`);
    if (res.data) {
      showNotification("success", res.data.message);
      await getOrderInfo();
    }
  } catch (error) {
    console.log(error.message);
  }
};

onMounted(async () => {
  await getPendingOrder();
});
</script>

<style lang="scss" scoped>
/* Custom styles for enhanced visual appeal */
.hover\:bg-gray-50:hover {
  transition: background-color 0.15s ease-in-out;
}

/* Custom scrollbar for table */
.overflow-x-auto::-webkit-scrollbar {
  height: 6px;
}

.overflow-x-auto::-webkit-scrollbar-track {
  background: #f1f5f9;
  border-radius: 3px;
}

.overflow-x-auto::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 3px;
}

.overflow-x-auto::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}

/* Enhanced button hover effects */
button {
  transition: all 0.2s ease-in-out;
}

/* Table row hover effect */
tr:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
}

/* Pagination styling */
:global(.ant-pagination) {
  display: flex;
  align-items: center;
}

:global(.ant-pagination .ant-pagination-item) {
  border-radius: 6px;
  border: 1px solid #d1d5db;
}

:global(.ant-pagination .ant-pagination-item-active) {
  background-color: #3b82f6;
  border-color: #3b82f6;
}

:global(.ant-pagination .ant-pagination-item-active a) {
  color: white;
}
</style>
