<template>
  <MainLayout>
    <div class="flex items-center justify-between">
      <button
        type="button"
        class="border px-4 py-1 rounded mr-2 bg-indigo-500 text-white"
        @click="$router.go(-1)"
      >
        Back
      </button>
      <button
        type="button"
        class="border px-4 py-1 rounded mr-2 bg-[#009688] text-white"
        @click="handlePrint"
        v-if="!loading"
      >
        Print
      </button>
    </div>
    <p v-if="loading">loading</p>
    <div class="bg-white printSection pr-3" ref="printSection" v-else>
      <div class="text-center">
        <div class="mb-1">
          <p class="uppercase font-bold font-size">Ousad bazar</p>
          <p>House-37, Block-F, Sector-1, Aftabnagar, Dhaka-1212</p>
          <p>01915606090</p>
        </div>
      </div>
      <div class="flex justify-between">
        <p>
          <strong>{{
            moment(OrderInfo?.sale_date).format("DD-MM-YYYY")
          }}</strong>
        </p>
        <p class="capitalize">Customer: {{ OrderInfo?.sold_user?.name }}</p>
      </div>
      <div class="flex justify-between">
        <p>Sale ID: {{ OrderInfo?.id }}</p>
        <p>Phone: {{ OrderInfo?.sold_user?.phone }}</p>
      </div>

      <div class="grid grid-cols-12 gap-x-3 border-t mt-2 border-black">
        <h6 class="font-bold col-span-5">Item Name</h6>
        <h6 class="font-bold text-right col-span-2">Price</h6>
        <h6 class="font-bold text-right col-span-2">Qty.</h6>
        <h6 class="font-bold text-right col-span-3 mr-1">Total</h6>
        <hr class="col-span-12 border-t border-black" />
        <template
          v-for="(product, index) in OrderInfo?.sale_products"
          :key="index"
        >
          <p class="col-span-5 whitespace-nowrap">
            {{ String(index + 1) }}.{{ product?.product_name?.slice(0, 14) }}
          </p>
          <p class="text-right col-span-2 justify-self-right">
            {{ Number(product?.ecom_final_selling_price)?.toFixed(2) }}
          </p>
          <p class="text-right col-span-2 justify-self-right">
            {{ Number(product?.total_quantity) }}
          </p>
          <p class="text-right mr-1 col-span-3 justify-self-right">
            {{ Number(product?.total)?.toFixed(2) }}
          </p>
          <hr class="col-span-12 border-t border-black border-dashed" />
        </template>

        <h6 class="font-bold col-span-9 text-right">Total</h6>
        <p class="text-right col-span-3 mr-1 justify-self-right">
          {{ Number(OrderInfo?.total).toFixed(2) }}
        </p>
      </div>

      <div class="max-w-fit mx-auto text-right">
        <h6 class="font-bold uppercase">
          Total:
          <span class="text-right">
            {{ Number(OrderInfo?.total).toFixed(2) }}
          </span>
        </h6>
      </div>
    </div>
  </MainLayout>
</template>

<script setup>
import MainLayout from "@/components/layouts/mainLayout.vue";
import moment from "moment";
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
// import { storeToRefs } from "pinia";
// import { useVerifyStore } from "@/stores/verify.js";
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

onMounted(() => {
  getOrderInfo();
});

const printSection = ref(null);

const handlePrint = () => {
  const printContent = printSection.value.innerHTML;
  document.body.innerHTML = printContent;
  window.print();
  window.location.reload();
};
</script>
<style scoped>
@media print {
  * {
    font-size: 8px !important;
    line-height: 1.6 !important;
  }
  .hide-print {
    display: none;
  }
  .font-size {
    font-size: 11px !important;
  }
  .text-right {
    text-align: right !important;
  }

  .justify-self-right {
    justify-self: right;
  }
}
.font-size {
  font-size: 20px;
}
.text-right {
  text-align: right !important;
}

.justify-self-right {
  justify-self: right;
}
</style>
