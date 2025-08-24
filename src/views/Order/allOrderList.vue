<template>
  <MainLayout>
    <div class="min-h-screen">
      <!-- Page Header -->
      <div
        class="bg-white rounded-xl shadow-sm border border-gray-200 p-6 mb-8"
      >
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-4">
            <div
              class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center"
            >
              <Icon
                icon="mdi:format-list-bulleted-square"
                class="w-6 h-6 text-blue-600"
              />
            </div>
            <div>
              <h1 class="text-2xl font-bold text-gray-900">All Orders</h1>
              <p class="text-sm text-gray-500 mt-1">
                Complete overview of all order transactions
              </p>
            </div>
          </div>
          <div class="flex items-center space-x-3">
            <span
              class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-blue-100 text-blue-800"
            >
              {{ backupData?.total || 0 }} Total Orders
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
          <div class="flex items-center justify-between">
            <h2 class="text-lg font-semibold text-gray-900 flex items-center">
              <Icon
                icon="mdi:clipboard-list"
                class="w-5 h-5 mr-2 text-gray-600"
              />
              Order Management
            </h2>
            <div class="text-sm text-gray-500">
              Page {{ currentPage }} of
              {{ Math.ceil((backupData?.total || 0) / (pageSize || 10)) }}
            </div>
          </div>
        </div>

        <!-- Loading State -->
        <div v-if="loading" class="p-12">
          <div class="flex flex-col items-center justify-center space-y-4">
            <Icon
              class="w-12 h-12 animate-spin text-blue-600"
              icon="mage:reload"
            />
            <h3 class="text-xl font-medium text-gray-900">Loading orders...</h3>
            <p class="text-gray-500">
              Please wait while we fetch the order data
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
                  Order Info
                </th>
                <th
                  class="px-6 py-4 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Order View
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Order Date
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Order Code
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Country
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  City
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Order Status
                </th>
                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Delivery Status
                </th>

                <th
                  class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  payment Status
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200">
              <tr
                v-for="(item, index) in AllOrder"
                :key="item.id"
                class="hover:bg-gray-50 transition-colors duration-200"
              >
                <!-- Order Info (SL + ID) -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center space-x-3">
                    <span
                      class="inline-flex items-center justify-center w-8 h-8 bg-blue-100 text-blue-800 text-sm font-medium rounded-full"
                    >
                      {{ (currentPage - 1) * (pageSize || 10) + index + 1 }}
                    </span>
                    <div>
                      <div class="text-sm font-medium text-gray-900">
                        Order #{{ item?.id }}
                      </div>
                      <div class="text-xs text-gray-500">
                        ID: {{ item?.id }}
                      </div>
                    </div>
                  </div>
                </td>

                <!-- Actions -->
                <td class="px-6 py-4 whitespace-nowrap text-center">
                  <router-link :to="{ name: 'view', params: { id: item?.id } }">
                    <button
                      class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-lg text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-all duration-200"
                      title="View Order Details"
                    >
                      <Icon icon="mdi:eye" class="w-4 h-4 mr-2" />
                    </button>
                  </router-link>
                </td>

                <!-- Order Date -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <div>
                      <div class="text-sm text-gray-900">
                        {{ formatDate(item?.sale_date) }}
                      </div>
                      <div class="text-xs text-gray-500">
                        {{ formatTime(item?.sale_date) }}
                      </div>
                    </div>
                  </div>
                </td>

                <!-- Order Code -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <span class="text-sm font-medium text-gray-900">
                      {{ item?.sale_code || "N/A" }}
                    </span>
                  </div>
                </td>

                <!-- Country -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <span class="text-sm text-gray-900">
                      {{ item?.billing_address?.country?.name || "N/A" }}
                    </span>
                  </div>
                </td>

                <!-- City -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="flex items-center">
                    <span class="text-sm text-gray-900">
                      {{ item?.billing_address?.city?.name || "N/A" }}
                    </span>
                  </div>
                </td>

                <!--Order Verify Status -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <span
                    class="inline-flex items-center px-2 py-1 rounded-lg text-xs font-medium"
                    :class="{
                      'bg-green-100 text-green-800': item?.verify_status == 1,
                      'bg-orange-100 text-orange-800': item?.verify_status == 0,
                      'bg-red-100 text-red-800': item?.verify_status == 2,
                    }"
                  >
                    {{
                      (item?.verify_status == 0 && "Pending") ||
                      (item?.verify_status == 1 && "Approved") ||
                      (item?.verify_status == 2 && "Cancel")
                    }}
                  </span>
                </td>
                <!-- Delivery Status -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <!-- {{ item?.delivery_status }} -->
                  <span
                    v-if="item?.delivery_status == 1"
                    class="inline-flex items-center px-2.5 py-0.5 rounded-lg text-xs font-medium bg-green-100 text-green-800"
                  >
                    Store Arrived
                  </span>
                  <span
                    v-else-if="item?.delivery_status == 2"
                    class="inline-flex items-center px-2.5 py-0.5 rounded-lg text-xs font-medium bg-green-100 text-green-800"
                  >
                    Start Journey
                  </span>
                  <span
                    v-else-if="item?.delivery_status == 3"
                    class="inline-flex items-center px-2.5 py-0.5 rounded-lg text-xs font-medium bg-green-100 text-green-800"
                  >
                    delivered
                  </span>

                  <span
                    v-else-if="item?.delivery_status == 4"
                    class="inline-flex items-center px-2.5 py-0.5 rounded-lg text-xs font-medium bg-orange-100 text-orange-800"
                  >
                    Not Reachable
                  </span>
                  <span
                    v-else-if="item?.delivery_status == 5"
                    class="inline-flex items-center px-2.5 py-0.5 rounded-lg text-xs font-medium bg-orange-100 text-orange-800"
                  >
                    Not Received
                  </span>
                </td>

                <!-- payment Status -->
                <td class="px-6 py-4 whitespace-nowrap">
                  <Span
                    class="text-white bg-red-600 px-2.5 py-1 rounded-lg text-xs font-medium"
                    v-if="
                      item?.delivery_status == 3 &&
                      item?.sale_transaction == null
                    "
                  >
                    Delivered but payment not transferred
                  </Span>
                  <a-popconfirm
                    v-else
                    title="Are you sure receive money?"
                    :disabled="item?.sale_transaction?.receive_status == 1"
                    @confirm="handleConfirm_Receive(item?.sale_transaction?.id)"
                    placement="top"
                  >
                    <button
                      class="inline-flex items-center px-2.5 py-0.5 rounded-lg text-xs font-medium"
                      :class="{
                        'bg-green-100 text-green-800':
                          item?.sale_transaction?.receive_status == 1,
                        'bg-orange-100 text-orange-800':
                          item?.sale_transaction?.receive_status == 0,
                      }"
                    >
                      {{
                        item?.sale_transaction
                          ? item?.sale_transaction?.receive_status == 1
                            ? "Received"
                            : "Not Received"
                          : "-"
                      }}
                    </button>
                  </a-popconfirm>
                </td>
              </tr>

              <!-- Empty State -->
              <tr v-if="!loading && (!AllOrder || AllOrder.length === 0)">
                <td colspan="7" class="px-6 py-12 text-center">
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
                        No orders found
                      </h3>
                      <p class="text-gray-500 mt-1">
                        There are no orders to display at the moment.
                      </p>
                    </div>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- Table Footer with Pagination -->
        <div
          v-if="!loading && AllOrder && AllOrder.length > 0"
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
import { showNotification } from "@/utilities/common";
import { Icon } from "@iconify/vue";
import axios from "axios";
import { onMounted, ref } from "vue";

const loading = ref(false);
const AllOrder = ref();
const backupData = ref();
const pageSize = ref(10);
const currentPage = ref(1);

const pagination = async (page) => {
  currentPage.value = page;
  await getAllOrder();
};

const handleConfirm_Receive = async (id) => {
  try {
    const res = await axios.post(
      `${apiBase}/admin/payment-received/${id}`,
      {
        receive_status: 1,
      },
      getTokenConfig()
    );
    if (res.data.status === "success") {
      showNotification("success", res.data.message);
      await getAllOrder();
    }
  } catch (error) {
    console.log(error.message);
  }
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
      pageSize.value = res?.data?.per_page || 10;
    }
  } catch (error) {
    console.log(error.message);
    loading.value = false;
  }
};

const formatDate = (dateString) => {
  if (!dateString) return "N/A";
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

onMounted(async () => {
  await getAllOrder();
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
