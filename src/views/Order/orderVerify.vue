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
              <h1 class="text-2xl font-bold text-gray-900">Order Processing</h1>
              <p class="text-sm text-gray-500 mt-1">Order ID: #{{ id }}</p>
            </div>
          </div>
          <div class="flex items-center space-x-3">
            <span
              class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-orange-100 text-orange-800"
            >
              Pending Verification
            </span>
          </div>
        </div>
      </div>

      <!-- Main Content -->
      <div class="space-y-8">
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
                          class="text-xs text-red-600 font-medium mt-1"
                        >
                          ⚠️ Low Stock Alert
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
                      product.quantity
                    }}</span>
                  </td>
                  <td class="px-6 py-4 text-center">
                    <span class="text-sm font-medium text-gray-900"
                      >৳{{ product.price.toLocaleString() }}</span
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
                        :src="`${imgBase}/${product?.products?.product_images[0].path}`"
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
              <div class="flex justify-between items-center py-2">
                <span class="text-gray-600">Subtotal</span>
                <span class="text-lg font-medium text-gray-900"
                  >৳{{ OrderInfo?.sub_total?.toLocaleString() }}</span
                >
              </div>

              <div class="flex justify-between items-center py-2">
                <span class="text-gray-600">Shipping Charge</span>
                <span class="text-lg font-medium text-gray-900"
                  >৳{{ OrderInfo?.shipping_cost?.toLocaleString() }}</span
                >
              </div>

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
                class="mt-6 p-4 bg-green-50 rounded-lg border border-green-200"
              >
                <div class="flex items-center justify-between">
                  <div class="flex items-center space-x-2">
                    <Icon
                      icon="mdi:credit-card"
                      class="w-5 h-5 text-green-600"
                    />
                    <span class="text-sm font-medium text-green-800"
                      >Payment Methods</span
                    >
                  </div>
                  <span
                    class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800"
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
          <div class="flex flex-col sm:flex-row gap-4 justify-between">
            <button
              @click="goBack"
              class="inline-flex items-center px-6 py-3 border border-gray-300 rounded-lg text-base font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-all duration-200"
            >
              <Icon icon="mdi:arrow-left" class="w-5 h-5 mr-2" />
              Go Back
            </button>
            <div class="flex gap-4 items-center">
              <button
                @click="verifyOrder"
                class="inline-flex items-center px-6 py-2.5 border border-transparent rounded-lg text-base font-medium text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 transition-all duration-200 shadow-sm"
              >
                <Icon icon="mdi:check-circle" class="w-5 h-5 mr-2" />
                Verify Order & Assign Delivery
              </button>

              <a-popconfirm
                title="Are you sure cancel this order?"
                ok-text="Yes, Cancel"
                cancel-text="No, Keep"
                @confirm="handleCancelOrder(OrderInfo.id)"
                @cancel="() => {}"
              >
                <button
                  class="rounded-lg text-base transition-colors px-6 py-2.5 inline-flex items-center"
                  :class="{
                    'bg-yellow-200  border-yellow-200 hover:bg-yellow-300':
                      OrderInfo?.verify_status == 0 ||
                      OrderInfo?.verify_status == null,
                    'bg-green-100 text-green-700 border-green-200 cursor-not-allowed':
                      OrderInfo?.verify_status == 1,
                    'bg-red-100 text-red-700 border-red-200 cursor-not-allowed':
                      OrderInfo?.verify_status == 2,
                  }"
                >
                  {{
                    OrderInfo?.customer_id === null
                      ? "Guest Order"
                      : "User Order"
                  }}

                  {{
                    OrderInfo?.verify_status == 0 ||
                    OrderInfo?.verify_status == null
                      ? "Pending"
                      : OrderInfo?.verify_status == 1
                      ? "Confirmed"
                      : "Cancelled"
                  }}
                </button>
              </a-popconfirm>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Delivery Assignment Modal -->
    <a-modal
      v-model:open="open"
      :footer="null"
      :width="600"
      class="delivery-modal"
    >
      <div class="p-6">
        <!-- Modal Header -->
        <div class="flex items-center space-x-3 mb-6">
          <div
            class="w-10 h-10 bg-blue-100 rounded-lg flex items-center justify-center"
          >
            <Icon icon="mdi:truck-delivery" class="w-5 h-5 text-blue-600" />
          </div>
          <div>
            <h2 class="text-xl font-bold text-gray-900">Assign Delivery Man</h2>
            <p class="text-sm text-gray-500">
              Select a delivery person for this order
            </p>
          </div>
        </div>

        <!-- Delivery Man Selection -->
        <div class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-2">
              Choose Delivery Person <span class="text-red-500">*</span>
            </label>
            <a-select
              v-model:value="delivery_man_id"
              show-search
              :filterOption="false"
              class="w-full"
              placeholder="Select a delivery person..."
              size="large"
            >
              <a-select-option
                v-for="info in deliveryMan_info"
                :key="info.id"
                :value="info.id"
              >
                <div class="flex items-center justify-start space-x-3 py-1">
                  <!-- <div
                    class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center"
                  >
                    <Icon icon="mdi:account" class="w-4 h-4 text-blue-600" />
                  </div> -->

                  <div>
                    <div class="font-medium text-gray-900">
                      {{ info?.name }}
                    </div>
                    <div class="text-xs text-gray-500">Delivery Agent</div>
                  </div>
                </div>
              </a-select-option>
            </a-select>
          </div>

          <!-- Order Summary in Modal -->
          <div class="bg-gray-50 rounded-lg p-4 border border-gray-200">
            <h3 class="text-sm font-medium text-gray-900 mb-3">
              Order Summary
            </h3>
            <div class="space-y-2 text-sm">
              <div class="flex justify-between">
                <span class="text-gray-600">Order ID:</span>
                <span class="font-medium">#{{ id }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-gray-600">Total Amount:</span>
                <span class="font-bold text-blue-600"
                  >৳{{ OrderInfo?.total?.toLocaleString() }}</span
                >
              </div>
              <div class="flex justify-between">
                <span class="text-gray-600">Items:</span>
                <span class="font-medium"
                  >{{ OrderInfo?.sale_products?.length }} products</span
                >
              </div>
            </div>
          </div>
        </div>

        <!-- Modal Actions -->
        <div
          class="flex justify-between items-center pt-6 border-t border-gray-200 mt-6"
        >
          <button
            @click="
              () => {
                open = false;
              }
            "
            class="inline-flex items-center px-4 py-2 border border-gray-300 rounded-lg text-sm font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition-all duration-200"
          >
            <Icon icon="mdi:close" class="w-4 h-4 mr-2" />
            Cancel
          </button>

          <a-popconfirm
            title="Are you sure you want to confirm this order assignment?"
            ok-text="Yes, Confirm"
            cancel-text="Cancel"
            @confirm="handleOrderConfirm"
          >
            <button
              class="inline-flex items-center px-6 py-2 border border-transparent rounded-lg text-sm font-medium text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 transition-all duration-200 shadow-sm"
              :disabled="!delivery_man_id"
              :class="{ 'opacity-50 cursor-not-allowed': !delivery_man_id }"
            >
              <Icon icon="mdi:check-circle" class="w-4 h-4 mr-2" />
              Confirm Order Assignment
            </button>
          </a-popconfirm>
        </div>
      </div>
    </a-modal>
  </MainLayout>
</template>

<script setup>
import { ref, onMounted } from "vue";
import MainLayout from "@/components/layouts/mainLayout.vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import { Icon } from "@iconify/vue";
import { apiBase, imgBase } from "@/config";
import { getTokenConfig } from "@/util/tokenConfig";
import { showNotification } from "@/utilities/common";

const route = useRoute();
const router = useRouter();
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

const open = ref(false);
const verifyOrder = () => {
  open.value = true;
};

const deliveryMan_info = ref([]);
const getDelivaryMan = async () => {
  try {
    const res = await axios.get(
      `${apiBase}/admin/delivery-man/search`,
      getTokenConfig()
    );
    if (res.data) {
      deliveryMan_info.value = res?.data;
    }
  } catch (error) {
    console.log(error.message);
  }
};

const delivery_man_id = ref();
const handleOrderConfirm = async () => {
  if (!delivery_man_id.value) {
    return showNotification("warning", "Please select delivery man");
  }
  try {
    const res = await axios.post(
      `${apiBase}/admin/sale/verified/${id}`,
      {
        delivery_man_id: delivery_man_id.value,
      },
      getTokenConfig()
    );

    if (res.data?.status === "success") {
      showNotification("success", res?.data?.message);
      open.value = false;
      delivery_man_id.value = "";
      router.push({ name: "pending-list" });
    } else {
      showNotification("error", res?.data?.message);
      open.value = false;
      delivery_man_id.value = "";
    }
    console.log(res.data);
  } catch (error) {
    console.log(error.message);
  }
};

const handleCancelOrder = async (id) => {
  try {
    const res = await axios.post(`${apiBase}/sale/request-to-suspend/${id}`);
    if (res.data) {
      showNotification("success", res.data.message);
      router.push({ name: "pending-list" });
    }
  } catch (error) {
    console.log(error.message);
  }
};

onMounted(async () => {
  await getDelivaryMan();
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

/* Modal styling */
:global(.delivery-modal .ant-modal-content) {
  border-radius: 12px;
  overflow: hidden;
}

:global(.delivery-modal .ant-modal-header) {
  border-bottom: 1px solid #e5e7eb;
  padding: 0;
}

:global(.delivery-modal .ant-modal-body) {
  padding: 0;
}

/* Select styling */
:global(.ant-select-selector) {
  border-radius: 8px !important;
  border: 1px solid #d1d5db !important;
}

:global(.ant-select-focused .ant-select-selector) {
  border-color: #3b82f6 !important;
  box-shadow: 0 0 0 2px rgb(59 130 246 / 0.1) !important;
}
</style>
