<template>
  <MainLayout>
    <div class="">
      <!-- Order Header -->
      <div class="flex justify-between items-center mb-6">
        <h2 class="text-2xl font-semibold text-gray-800">Order View</h2>
      </div>

      <!-- Product Details (Multiple Products) -->
      <div class="space-y-4">
        <div class="space-y-4">
          <h3 class="text-xl font-semibold text-gray-800 mb-4">
            Product Details
          </h3>
          <table class="min-w-full border mb-4">
            <thead>
              <tr>
                <th class="px-2 py-1 text-left text-gray-700">Product</th>
                <th class="px-2 py-1 text-center text-gray-700">
                  Stock Quantity
                </th>
                <th class="px-2 py-1 text-center text-gray-700">Quantity</th>
                <th class="px-2 py-1 text-center text-gray-700">Price</th>
                <th class="px-2 py-1 text-center text-gray-700">Total</th>
                <th class="px-2 py-1 text-center text-gray-700">Image</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(product, index) in OrderInfo?.sale_products"
                :key="index"
                class="border-t"
                :class="{
                  'bg-red-200':
                    stockQuantity(product?.products) <= product.quantity,
                }"
              >
                <td class="px-2 py-1 text-gray-700">
                  {{ product.product_name }}
                </td>
                <td class="px-2 py-1 text-gray-700 text-center">
                  <!-- {{ product?.products?.stock_batches[0]?.balanced_quantity }} -->
                  <!-- {{ product }} -->
                  {{ stockQuantity(product?.products) }}
                </td>
                <td class="px-2 py-1 text-gray-700 text-center">
                  {{ product.quantity }}
                </td>
                <td class="px-2 py-1 text-gray-700 text-center">
                  ৳ {{ product.price }}
                </td>
                <td class="px-2 py-1 text-gray-700 text-center">
                  ৳ {{ product.total }}
                </td>
                <td class="px-2 py-1 flex justify-center">
                  <img
                    :src="`${imgBase}/${product?.products?.product_images[0].path}`"
                    alt="Product Image"
                    class="w-24 h-24 object-cover rounded-md"
                  />
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- Billing Address -->
        <div class="flex justify-between items-center gap-5">
          <div class="bg-gray-50 p-4 rounded-lg shadow-sm mb-4 w-full">
            <h3 class="text-xl font-semibold text-gray-800 mb-4">
              Billing Address
            </h3>
            <p class="text-gray-700">
              <strong>Name:</strong>
              {{ OrderInfo?.billing_address?.full_name || "N/A" }}
            </p>
            <p class="text-gray-700">
              <strong>Address:</strong>
              {{ OrderInfo?.billing_address?.address || "N/A" }}
            </p>
            <p class="text-gray-700">
              <strong>Mobile:</strong>
              {{ OrderInfo?.billing_address?.mobile || "N/A" }}
            </p>
            <p class="text-gray-700">
              <strong>City:</strong>
              {{ OrderInfo?.billing_address?.city?.name || "N/A" }}
            </p>
            <p class="text-gray-700">
              <strong>Country:</strong>
              {{ OrderInfo?.billing_address?.country?.name || "N/A" }}
            </p>
          </div>

          <!-- Order Summary -->
          <div class="bg-gray-50 p-4 rounded-lg shadow-sm mb-4 w-full">
            <h3 class="text-xl font-semibold text-gray-800 mb-4">
              Order Summary
            </h3>
            <div class="flex justify-between mb-2">
              <p class="text-gray-700">Subtotal:</p>
              <p class="text-gray-700 font-bold">
                ৳ {{ OrderInfo?.sub_total }}
              </p>
            </div>
            <div class="flex justify-between mb-2">
              <p class="text-gray-700">Shipping Charge:</p>
              <p class="text-gray-700 font-bold">
                ৳ {{ OrderInfo?.shipping_cost }}
              </p>
            </div>
            <div class="flex justify-between mt-4 border-t pt-4">
              <p class="text-gray-700">Total:</p>
              <p class="text-gray-700 font-bold">৳ {{ OrderInfo?.total }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Action Buttons -->
      <div class="mt-6 flex justify-between">
        <button
          class="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition duration-300"
          @click="goBack"
        >
          Back
        </button>
        <a-popconfirm
          title="Are you sure delete this task?"
          ok-text="Yes"
          cancel-text="No"
          @confirm="verifyOrder"
        >
          <button
            class="px-6 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition duration-300"
          >
            Verify
          </button>
        </a-popconfirm>
      </div>
    </div>
  </MainLayout>
  <a-modal :footer="null" v-model:open="open" @ok="handleOk">
    <div class="space-y-4">
      <h2 class="text-xl font-bold">Assign delivery man</h2>
      <!-- @search="searchHandle($event)" -->

      <div class="w-full">
        <a-select
          v-model:value="delivery_man_id"
          show-search
          :filterOption="false"
          class="w-full capitalize"
        >
          <a-select-option v-for="info in deliveryMan_info" :key="info.id">
            <h3 class="font-bold text-lg">{{ info?.name }}</h3>
          </a-select-option>
        </a-select>
      </div>
      <a-popconfirm
        title="Are you sure Confirm this Order?"
        ok-text="Yes"
        cancel-text="No"
        @confirm="handleOrderConfirm"
      >
        <button class="border px-6 py-1 rounded bg-green-500 text-white">
          Confirm
        </button>
      </a-popconfirm>
    </div>
  </a-modal>
</template>

<script setup>
import { ref, onMounted } from "vue";
import MainLayout from "@/components/layouts/mainLayout.vue";
import { useRoute } from "vue-router";
import axios from "axios";
import { apiBase, imgBase } from "@/config";
import { getTokenConfig } from "@/util/tokenConfig";
import { showNotification } from "@/utilities/common";

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
    } else {
      showNotification("error", res?.data?.message);
    }
    console.log(res.data);
  } catch (error) {
    console.log(error.message);
  }
};

onMounted(async () => {
  await getDelivaryMan();
});
</script>

<style lang="scss" scoped>
/* Custom styles for this component */
</style>
