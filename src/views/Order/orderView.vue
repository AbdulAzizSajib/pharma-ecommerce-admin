<template>
  <MainLayout>
    <div class="min-h-screen">
      <!-- Order Header -->
      <div
        class="bg-white rounded-xl shadow-sm border border-gray-200 p-6 mb-8"
      >
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-4">
            <div
              class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center"
            >
              <Icon icon="mdi:receipt-text" class="w-6 h-6 text-blue-600" />
            </div>
            <div>
              <h1 class="text-2xl font-bold text-gray-900">Order Details</h1>
              <p class="text-sm text-gray-500 mt-1">Order ID: #{{ id }}</p>
            </div>
          </div>
          <div class="flex items-center space-x-3">
            <span
              class="inline-flex items-center px-3 py-1 rounded-2xl text-sm font-medium bg-green-100 text-green-800"
            >
              Active Order
            </span>
          </div>
        </div>
      </div>

      <!-- Loading State -->
      <div
        v-if="loading"
        class="bg-white rounded-xl shadow-sm border border-gray-200 p-12"
      >
        <div class="flex flex-col items-center justify-center space-y-4">
          <Icon
            class="w-12 h-12 animate-spin text-blue-600"
            icon="mage:reload"
          />
          <h3 class="text-xl font-medium text-gray-900">
            Loading order details...
          </h3>
          <p class="text-gray-500">
            Please wait while we fetch your order information
          </p>
        </div>
      </div>

      <!-- Main Content -->
      <div v-else class="space-y-8">
        <!-- Product Details Section -->
        <div
          class="bg-white rounded-xl shadow-sm border border-gray-200 overflow-hidden"
        >
          <div class="px-6 py-4 border-b border-gray-200 bg-gray-50">
            <h2 class="text-lg font-semibold text-gray-900 flex items-center">
              <Icon
                icon="mdi:package-variant"
                class="w-5 h-5 mr-2 text-gray-600"
              />
              Product Details
            </h2>
          </div>

          <div class="overflow-x-auto">
            <table class="w-full">
              <thead class="bg-gray-50 border-b border-gray-200">
                <tr>
                  <th
                    class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                  >
                    Product
                  </th>
                  <th
                    class="px-6 py-4 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                  >
                    Stock
                  </th>
                  <th
                    class="px-6 py-4 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                  >
                    Quantity
                  </th>
                  <th
                    class="px-6 py-4 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                  >
                    Price
                  </th>
                  <th
                    class="px-6 py-4 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                  >
                    Total
                  </th>
                  <th
                    class="px-6 py-4 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                  >
                    Image
                  </th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-200">
                <tr
                  v-for="(product, index) in OrderInfo?.sale_products"
                  :key="index"
                  class="hover:bg-gray-50 transition-colors duration-200"
                  :class="{
                    'bg-red-50 border-l-4 border-l-red-400':
                      stockQuantity(product?.products) <= product.quantity,
                  }"
                >
                  <td class="px-6 py-4">
                    <div class="flex items-center">
                      <div>
                        <div class="text-sm font-medium text-gray-900">
                          {{ product.product_name }}
                        </div>
                        <div
                          v-if="
                            stockQuantity(product?.products) <= product.quantity
                          "
                          class="text-xs text-red-600 font-medium mt-1 flex items-center gap-2"
                        >
                          <Icon class="size-4" icon="emojione:warning" /> Low
                          Stock Alert
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 text-center">
                    <span
                      class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium"
                      :class="{
                        'bg-red-100 text-red-800':
                          stockQuantity(product?.products) <= product.quantity,
                        'bg-green-100 text-green-800':
                          stockQuantity(product?.products) > product.quantity,
                      }"
                    >
                      {{ stockQuantity(product?.products) }}
                    </span>
                  </td>
                  <td class="px-6 py-4 text-center">
                    <span class="text-sm font-medium text-gray-900">{{
                      product?.total_quantity
                    }}</span>
                  </td>
                  <td class="px-6 py-4 text-center">
                    <span class="text-sm font-medium text-gray-900"
                      >৳{{
                        product.ecom_final_selling_price.toLocaleString()
                      }}</span
                    >
                  </td>
                  <td class="px-6 py-4 text-center">
                    <span class="text-sm font-bold text-gray-900"
                      >৳{{ product.total.toLocaleString() }}</span
                    >
                  </td>
                  <td class="px-6 py-4 text-center">
                    <div class="flex justify-center">
                      <img
                        :src="`${imgBase}/${product?.products?.product_images[0]?.path}`"
                        alt="Product Image"
                        class="w-16 h-16 object-cover rounded-lg border border-gray-200 shadow-sm"
                      />
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Address and Summary Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
          <!-- Billing Address -->
          <div class="bg-white rounded-xl shadow-sm border border-gray-200">
            <div class="px-6 py-4 border-b border-gray-200 bg-gray-50">
              <h2 class="text-lg font-semibold text-gray-900 flex items-center">
                <Icon
                  icon="mdi:map-marker"
                  class="w-5 h-5 mr-2 text-gray-600"
                />
                Billing Address
              </h2>
            </div>
            <div class="p-6 space-y-4">
              <div class="flex items-start space-x-3">
                <Icon icon="mdi:account" class="w-5 h-5 text-gray-400 mt-0.5" />
                <div>
                  <p class="text-sm text-gray-500">Full Name</p>
                  <p class="text-base font-medium text-gray-900">
                    {{ OrderInfo?.billing_address?.full_name || "N/A" }}
                  </p>
                </div>
              </div>

              <div class="flex items-start space-x-3">
                <Icon icon="mdi:home" class="w-5 h-5 text-gray-400 mt-0.5" />
                <div>
                  <p class="text-sm text-gray-500">Address</p>
                  <p class="text-base font-medium text-gray-900">
                    {{ OrderInfo?.billing_address?.address || "N/A" }}
                  </p>
                </div>
              </div>

              <div class="flex items-start space-x-3">
                <Icon icon="mdi:phone" class="w-5 h-5 text-gray-400 mt-0.5" />
                <div>
                  <p class="text-sm text-gray-500">Mobile</p>
                  <p class="text-base font-medium text-gray-900">
                    {{ OrderInfo?.billing_address?.mobile || "N/A" }}
                  </p>
                </div>
              </div>

              <div class="flex items-start space-x-3">
                <Icon icon="mdi:city" class="w-5 h-5 text-gray-400 mt-0.5" />
                <div>
                  <p class="text-sm text-gray-500">City</p>
                  <p class="text-base font-medium text-gray-900">
                    {{ OrderInfo?.billing_address?.city?.name || "N/A" }}
                  </p>
                </div>
              </div>

              <div class="flex items-start space-x-3">
                <Icon icon="mdi:earth" class="w-5 h-5 text-gray-400 mt-0.5" />
                <div>
                  <p class="text-sm text-gray-500">Country</p>
                  <p class="text-base font-medium text-gray-900">
                    {{ OrderInfo?.billing_address?.country?.name || "N/A" }}
                  </p>
                </div>
              </div>
            </div>
          </div>

          <!-- Order Summary -->
          <div class="bg-white rounded-xl shadow-sm border border-gray-200">
            <div class="px-6 py-4 border-b border-gray-200 bg-gray-50">
              <h2 class="text-lg font-semibold text-gray-900 flex items-center">
                <Icon
                  icon="mdi:calculator"
                  class="w-5 h-5 mr-2 text-gray-600"
                />
                Order Summary
              </h2>
            </div>
            <div class="p-6 space-y-4">
              <!-- <div class="flex justify-between items-center py-2">
                <span class="text-gray-600">Subtotal</span>
                <span class="text-lg font-medium text-gray-900"
                  >৳{{ OrderInfo?.sub_total?.toLocaleString() }}</span
                >
              </div> -->

              <div class="border-t border-gray-200 pt-4">
                <div class="flex justify-between items-center">
                  <span class="text-lg font-semibold text-gray-900"
                    >Total Amount</span
                  >
                  <span class="text-2xl font-bold text-blue-600"
                    >৳{{ OrderInfo?.total?.toLocaleString() }}</span
                  >
                </div>
              </div>

              <!-- Payment Status -->
              <div
                class="mt-6 p-4 bg-blue-50 rounded-lg border border-blue-200"
              >
                <div class="flex items-center justify-between">
                  <div class="flex items-center space-x-2">
                    <Icon
                      icon="mdi:credit-card"
                      class="w-5 h-5 text-blue-600"
                    />
                    <span class="text-sm font-medium text-blue-800"
                      >Payment Method</span
                    >
                  </div>
                  <span
                    class="inline-flex items-center px-2.5 py-0.5 rounded-2xl font-bold text-xs bg-blue-100 text-blue-800"
                  >
                    {{ OrderInfo?.payment_method?.name }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Action Buttons -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <div class="flex flex-col sm:flex-row gap-4 justify-center">
            <button
              @click="goBack"
              class="inline-flex items-center px-6 py-3 border border-gray-300 rounded-lg text-base font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-all duration-200"
            >
              <Icon icon="mdi:arrow-left" class="w-5 h-5 mr-2" />
              Go Back
            </button>

            <router-link
              :to="{ name: 'receipt', params: { id: OrderInfo?.id } }"
            >
              <button
                class="inline-flex items-center px-6 py-3 border border-gray-300 rounded-lg text-base font-medium text-gray-700"
              >
                <Icon
                  icon="material-symbols-light:print-rounded"
                  class="size-8 mr-2"
                />
                Print
              </button>
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </MainLayout>
</template>

<script setup>
import { ref, onMounted } from "vue";
import MainLayout from "@/components/layouts/mainLayout.vue";
import { useRoute } from "vue-router";
import axios from "axios";
import { Icon } from "@iconify/vue";
import { apiBase, imgBase } from "@/config";
import { getTokenConfig } from "@/util/tokenConfig";

const route = useRoute();
const loading = ref(false);
const id = route.params.id;
const OrderInfo = ref(null);

// Fetch order details by ID
const getOrderInfo = async () => {
  loading.value = true;
  try {
    const res = await axios.get(
      `${apiBase}/admin/sales/${id}`,
      getTokenConfig()
    );
    loading.value = false;
    if (res?.data) {
      OrderInfo.value = res?.data?.sale;
    }
  } catch (error) {
    console.error("Error fetching order info:", error.message);
    loading.value = false;
  }
};

const stockQuantity = (item) => {
  if (!item?.stock_batches?.length) return 0;
  const stock = item?.stock_batches?.reduce((sum, batch) => {
    return sum + parseFloat(batch.balanced_quantity || 0);
  }, 0);
  return stock || 0;
};

onMounted(() => {
  getOrderInfo();
});

// Methods for buttons
const goBack = () => {
  window.history.back();
};
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
</style>
