<template>
  <MainLayout>
    <div class="p-6 bg-white rounded-lg border">
      <!-- Order Header -->
      <div class="flex justify-between items-center mb-6">
        <h2 class="text-2xl font-semibold text-gray-800">Order View</h2>
      </div>

      <!-- Product Details (Multiple Products) -->
      <div class="space-y-4">
        <h3 class="text-xl font-semibold text-gray-800 mb-4">
          Product Details
        </h3>

        <div
          v-for="(product, index) in products"
          :key="index"
          class="bg-gray-50 p-4 rounded-lg shadow-sm mb-4"
        >
          <div class="grid grid-cols-3 gap-4">
            <div class="col-span-2">
              <p class="text-gray-700">
                <strong>Product:</strong> {{ product.name }}
              </p>
              <p class="text-gray-700">
                <strong>Stock:</strong> {{ product.stock }} units
              </p>
              <p class="text-gray-700">
                <strong>Quantity:</strong> {{ product.quantity }}
              </p>
            </div>
            <div class="flex justify-end">
              <img
                :src="product.image"
                alt="Product Image"
                class="w-20 h-20 object-cover rounded-md"
              />
            </div>
          </div>
        </div>

        <!-- Billing Address -->
        <div class="bg-gray-50 p-4 rounded-lg shadow-sm mb-4">
          <h3 class="text-xl font-semibold text-gray-800 mb-4">
            Billing Address
          </h3>
          <p class="text-gray-700">John Doe</p>
          <p class="text-gray-700">123 Example Street</p>
          <p class="text-gray-700">City, State, 12345</p>
          <p class="text-gray-700">Country: Example Country</p>
        </div>

        <!-- Order Summary -->
        <div class="bg-gray-50 p-4 rounded-lg shadow-sm mb-4">
          <h3 class="text-xl font-semibold text-gray-800 mb-4">
            Order Summary
          </h3>
          <div class="flex justify-between mb-2">
            <p class="text-gray-700">Subtotal:</p>
            <p class="text-gray-700 font-bold">{{ subtotal }} Taka</p>
          </div>
          <div class="flex justify-between mb-2">
            <p class="text-gray-700">Shipping Charge:</p>
            <p class="text-gray-700 font-bold">{{ shippingCharge }} Taka</p>
          </div>
          <div class="flex justify-between mt-4 border-t pt-4">
            <p class="text-gray-700">Total:</p>
            <p class="text-gray-700 font-bold">{{ total }} Taka</p>
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
        <button
          class="px-6 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition duration-300"
          @click="verifyOrder"
        >
          Verify
        </button>
      </div>
    </div>
  </MainLayout>
</template>

<script setup>
import { ref, computed } from "vue";
import MainLayout from "@/components/layouts/mainLayout.vue";

// Sample data for products
const products = ref([
  {
    name: "Example Product 1",
    stock: 50,
    quantity: 2,
    image: "https://via.placeholder.com/100",
  },
  {
    name: "Example Product 2",
    stock: 30,
    quantity: 1,
    image: "https://via.placeholder.com/100",
  },
]);

// Compute subtotal, shipping charge, and total
const subtotal = computed(() => {
  return products.value.reduce((total, product) => {
    return total + product.quantity * 500; // Assuming each product costs 500 Taka
  }, 0);
});

const shippingCharge = 100; // Fixed shipping charge
const total = computed(() => {
  return subtotal.value + shippingCharge;
});

// Methods for buttons
const goBack = () => {
  window.history.back();
};

const verifyOrder = () => {
  alert("Order Verified");
};
</script>

<style lang="scss" scoped>
/* Additional custom styles can go here */
</style>
