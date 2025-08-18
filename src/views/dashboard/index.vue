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
              <Icon icon="mdi:view-dashboard" class="w-6 h-6 text-blue-600" />
            </div>
            <div>
              <h1 class="text-2xl font-bold text-gray-900">Admin Dashboard</h1>
              <p class="text-sm text-gray-500 mt-1">
                Welcome back! Here's what's happening with your store today.
              </p>
            </div>
          </div>
          <div class="flex items-center space-x-3">
            <span
              class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-green-100 text-green-800"
            >
              <Icon icon="mdi:circle" class="w-2 h-2 mr-2 animate-pulse" />
              System Online
            </span>
          </div>
        </div>
      </div>

      <!-- Revenue Stats -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <!-- Today's Revenue -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm font-medium text-gray-500">Today's Revenue</p>
              <p class="text-2xl font-bold text-gray-900 mt-1">৳1,000</p>
              <p class="text-xs text-green-600 flex items-center mt-2">
                <Icon icon="mdi:trending-up" class="w-3 h-3 mr-1" />
                +12% from yesterday
              </p>
            </div>
            <div
              class="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center"
            >
              <Icon icon="mdi:currency-bdt" class="w-6 h-6 text-green-600" />
            </div>
          </div>
        </div>

        <!-- Monthly Revenue -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm font-medium text-gray-500">Monthly Revenue</p>
              <p class="text-2xl font-bold text-gray-900 mt-1">৳35,000</p>
              <p class="text-xs text-blue-600 flex items-center mt-2">
                <Icon icon="mdi:calendar-month" class="w-3 h-3 mr-1" />
                This month
              </p>
            </div>
            <div
              class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center"
            >
              <Icon icon="mdi:chart-line" class="w-6 h-6 text-blue-600" />
            </div>
          </div>
        </div>

        <!-- Total Orders -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm font-medium text-gray-500">Total Orders</p>
              <p class="text-2xl font-bold text-gray-900 mt-1">1,247</p>
              <p class="text-xs text-purple-600 flex items-center mt-2">
                <Icon icon="mdi:package-variant" class="w-3 h-3 mr-1" />
                All time
              </p>
            </div>
            <div
              class="w-12 h-12 bg-purple-100 rounded-lg flex items-center justify-center"
            >
              <Icon icon="mdi:shopping" class="w-6 h-6 text-purple-600" />
            </div>
          </div>
        </div>

        <!-- Pending Orders -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="text-sm font-medium text-gray-500">Pending Orders</p>
              <p class="text-2xl font-bold text-gray-900 mt-1">100</p>
              <p class="text-xs text-orange-600 flex items-center mt-2">
                <Icon icon="mdi:clock-outline" class="w-3 h-3 mr-1" />
                Needs attention
              </p>
            </div>
            <div
              class="w-12 h-12 bg-orange-100 rounded-lg flex items-center justify-center"
            >
              <Icon icon="mdi:clock-alert" class="w-6 h-6 text-orange-600" />
            </div>
          </div>
        </div>
      </div>

      <!-- Quick Actions -->
      <div
        class="bg-white rounded-xl shadow-sm border border-gray-200 p-6 mb-8"
      >
        <h2 class="text-lg font-semibold text-gray-900 mb-6 flex items-center">
          <Icon icon="mdi:lightning-bolt" class="w-5 h-5 mr-2 text-gray-600" />
          Quick Actions
        </h2>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          <!-- Pending Orders Card -->
          <router-link to="/pending-orders" class="group">
            <div
              class="bg-gradient-to-br from-red-50 to-red-100 rounded-xl p-6 border border-red-200 hover:shadow-md transition-all duration-200 group-hover:scale-105"
            >
              <div class="flex items-center space-x-4">
                <div
                  class="w-12 h-12 bg-red-600 rounded-lg flex items-center justify-center"
                >
                  <Icon icon="mdi:clock-alert" class="w-6 h-6 text-white" />
                </div>
                <div>
                  <h3 class="text-lg font-semibold text-gray-900">
                    Pending Orders
                  </h3>
                  <p class="text-sm text-gray-600">
                    View & process pending orders
                  </p>
                  <p class="text-2xl font-bold text-red-600 mt-2">100 orders</p>
                </div>
              </div>
            </div>
          </router-link>

          <!-- All Orders Card -->
          <router-link to="/all-orders" class="group">
            <div
              class="bg-gradient-to-br from-blue-50 to-blue-100 rounded-xl p-6 border border-blue-200 hover:shadow-md transition-all duration-200 group-hover:scale-105"
            >
              <div class="flex items-center space-x-4">
                <div
                  class="w-12 h-12 bg-blue-600 rounded-lg flex items-center justify-center"
                >
                  <Icon
                    icon="mdi:format-list-bulleted"
                    class="w-6 h-6 text-white"
                  />
                </div>
                <div>
                  <h3 class="text-lg font-semibold text-gray-900">
                    All Orders
                  </h3>
                  <p class="text-sm text-gray-600">Manage all order history</p>
                  <p class="text-2xl font-bold text-blue-600 mt-2">
                    1,247 total
                  </p>
                </div>
              </div>
            </div>
          </router-link>

          <!-- Products Card -->
          <router-link to="/products" class="group">
            <div
              class="bg-gradient-to-br from-green-50 to-green-100 rounded-xl p-6 border border-green-200 hover:shadow-md transition-all duration-200 group-hover:scale-105"
            >
              <div class="flex items-center space-x-4">
                <div
                  class="w-12 h-12 bg-green-600 rounded-lg flex items-center justify-center"
                >
                  <Icon icon="mdi:package-variant" class="w-6 h-6 text-white" />
                </div>
                <div>
                  <h3 class="text-lg font-semibold text-gray-900">Products</h3>
                  <p class="text-sm text-gray-600">Manage product inventory</p>
                  <p class="text-2xl font-bold text-green-600 mt-2">
                    543 items
                  </p>
                </div>
              </div>
            </div>
          </router-link>

          <!-- Customers Card -->
          <router-link to="/customers" class="group">
            <div
              class="bg-gradient-to-br from-purple-50 to-purple-100 rounded-xl p-6 border border-purple-200 hover:shadow-md transition-all duration-200 group-hover:scale-105"
            >
              <div class="flex items-center space-x-4">
                <div
                  class="w-12 h-12 bg-purple-600 rounded-lg flex items-center justify-center"
                >
                  <Icon icon="mdi:account-group" class="w-6 h-6 text-white" />
                </div>
                <div>
                  <h3 class="text-lg font-semibold text-gray-900">Customers</h3>
                  <p class="text-sm text-gray-600">Manage customer accounts</p>
                  <p class="text-2xl font-bold text-purple-600 mt-2">
                    892 users
                  </p>
                </div>
              </div>
            </div>
          </router-link>
        </div>
      </div>

      <!-- Recent Activity & Analytics -->
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <!-- Recent Orders -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <h2
            class="text-lg font-semibold text-gray-900 mb-6 flex items-center"
          >
            <Icon icon="mdi:clock-outline" class="w-5 h-5 mr-2 text-gray-600" />
            Recent Orders
          </h2>

          <div class="space-y-4">
            <div
              class="flex items-center justify-between p-4 bg-gray-50 rounded-lg"
            >
              <div class="flex items-center space-x-3">
                <div
                  class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center"
                >
                  <Icon icon="mdi:shopping" class="w-4 h-4 text-blue-600" />
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-900">Order #12847</p>
                  <p class="text-xs text-gray-500">John Doe • 2 items</p>
                </div>
              </div>
              <div class="text-right">
                <p class="text-sm font-bold text-gray-900">৳1,200</p>
                <span
                  class="inline-flex items-center px-2 py-1 rounded-full text-xs font-medium bg-green-100 text-green-800"
                >
                  Completed
                </span>
              </div>
            </div>

            <div
              class="flex items-center justify-between p-4 bg-gray-50 rounded-lg"
            >
              <div class="flex items-center space-x-3">
                <div
                  class="w-8 h-8 bg-orange-100 rounded-full flex items-center justify-center"
                >
                  <Icon
                    icon="mdi:clock-outline"
                    class="w-4 h-4 text-orange-600"
                  />
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-900">Order #12846</p>
                  <p class="text-xs text-gray-500">Jane Smith • 1 item</p>
                </div>
              </div>
              <div class="text-right">
                <p class="text-sm font-bold text-gray-900">৳800</p>
                <span
                  class="inline-flex items-center px-2 py-1 rounded-full text-xs font-medium bg-orange-100 text-orange-800"
                >
                  Pending
                </span>
              </div>
            </div>

            <div
              class="flex items-center justify-between p-4 bg-gray-50 rounded-lg"
            >
              <div class="flex items-center space-x-3">
                <div
                  class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center"
                >
                  <Icon
                    icon="mdi:truck-delivery"
                    class="w-4 h-4 text-blue-600"
                  />
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-900">Order #12845</p>
                  <p class="text-xs text-gray-500">Mike Johnson • 3 items</p>
                </div>
              </div>
              <div class="text-right">
                <p class="text-sm font-bold text-gray-900">৳1,500</p>
                <span
                  class="inline-flex items-center px-2 py-1 rounded-full text-xs font-medium bg-blue-100 text-blue-800"
                >
                  Shipping
                </span>
              </div>
            </div>
          </div>

          <div class="mt-4 pt-4 border-t border-gray-200">
            <router-link
              to="/all-orders"
              class="text-sm text-blue-600 hover:text-blue-700 font-medium flex items-center"
            >
              View all orders
              <Icon icon="mdi:arrow-right" class="w-4 h-4 ml-1" />
            </router-link>
          </div>
        </div>

        <!-- System Status -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
          <h2
            class="text-lg font-semibold text-gray-900 mb-6 flex items-center"
          >
            <Icon icon="mdi:chart-bar" class="w-5 h-5 mr-2 text-gray-600" />
            System Overview
          </h2>

          <div class="space-y-6">
            <!-- Order Processing Rate -->
            <div>
              <div class="flex items-center justify-between mb-2">
                <span class="text-sm font-medium text-gray-700"
                  >Order Processing Rate</span
                >
                <span class="text-sm text-gray-900 font-bold">92%</span>
              </div>
              <div class="w-full bg-gray-200 rounded-full h-2">
                <div
                  class="bg-green-600 h-2 rounded-full"
                  style="width: 92%"
                ></div>
              </div>
            </div>

            <!-- Inventory Status -->
            <div>
              <div class="flex items-center justify-between mb-2">
                <span class="text-sm font-medium text-gray-700"
                  >Inventory Health</span
                >
                <span class="text-sm text-gray-900 font-bold">78%</span>
              </div>
              <div class="w-full bg-gray-200 rounded-full h-2">
                <div
                  class="bg-blue-600 h-2 rounded-full"
                  style="width: 78%"
                ></div>
              </div>
            </div>

            <!-- Customer Satisfaction -->
            <div>
              <div class="flex items-center justify-between mb-2">
                <span class="text-sm font-medium text-gray-700"
                  >Customer Satisfaction</span
                >
                <span class="text-sm text-gray-900 font-bold">96%</span>
              </div>
              <div class="w-full bg-gray-200 rounded-full h-2">
                <div
                  class="bg-purple-600 h-2 rounded-full"
                  style="width: 96%"
                ></div>
              </div>
            </div>

            <!-- Low Stock Alert -->
            <div class="bg-orange-50 border border-orange-200 rounded-lg p-4">
              <div class="flex items-center">
                <Icon
                  icon="mdi:alert-circle"
                  class="w-5 h-5 text-orange-600 mr-2"
                />
                <div>
                  <p class="text-sm font-medium text-orange-800">
                    Low Stock Alert
                  </p>
                  <p class="text-xs text-orange-600">
                    12 products need restocking
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </MainLayout>
</template>

<script setup>
import { Icon } from "@iconify/vue";
import MainLayout from "@/components/layouts/mainLayout.vue";
</script>

<style scoped>
/* Custom hover effects for quick action cards */
.group:hover .group-hover\:scale-105 {
  transform: scale(1.02);
}

/* Progress bar animations */
@keyframes progressBar {
  from {
    width: 0%;
  }
  to {
    width: var(--progress-width);
  }
}

/* Custom transitions */
.transition-all {
  transition: all 0.2s ease-in-out;
}

/* Card hover effects */
.hover\:shadow-md:hover {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
}
</style>
