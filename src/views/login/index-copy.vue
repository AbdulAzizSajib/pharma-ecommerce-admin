<template>
  <section
    class="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-600 via-purple-600 to-blue-800 p-5 relative"
  >
    <!-- Background overlay -->
    <div class="absolute inset-0 bg-black/10"></div>

    <div class="relative z-10 w-full max-w-md">
      <div
        class="bg-white/95 backdrop-blur-sm rounded-2xl shadow-2xl p-8 border border-white/20"
      >
        <!-- Header Section -->
        <div class="text-center mb-8">
          <div class="mb-6">
            <!-- <img
              src="@/assets/images/logo.png"
              class="h-16 w-auto max-w-48 mx-auto"
              alt="Pharma Admin"
            /> -->
          </div>
          <h1 class="text-3xl font-bold text-gray-900 mb-2 tracking-tight">
            Admin Portal
          </h1>
          <p class="text-gray-600 text-base">Sign in to your account</p>
        </div>

        <!-- Form Section -->
        <a-form
          :model="form"
          layout="vertical"
          @finish="handleLogin"
          class="space-y-4"
        >
          <a-form-item
            name="email"
            :rules="[
              { required: true, message: 'Email is required' },
              { type: 'email', message: 'Please enter a valid email' },
            ]"
          >
            <template #label>
              <span class="text-sm font-semibold text-gray-700"
                >Email Address</span
              >
            </template>
            <a-input
              v-model:value="form.email"
              placeholder="Enter your email"
              size="large"
              :disabled="isLoading"
              class="rounded-lg border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 transition-all duration-200"
            >
              <template #prefix>
                <UserOutlined class="text-gray-400 text-base" />
              </template>
            </a-input>
          </a-form-item>

          <a-form-item
            name="password"
            :rules="[
              { required: true, message: 'Password is required' },
              { min: 6, message: 'Password must be at least 6 characters' },
            ]"
          >
            <template #label>
              <span class="text-sm font-semibold text-gray-700">Password</span>
            </template>
            <a-input-password
              v-model:value="form.password"
              placeholder="Enter your password"
              size="large"
              :disabled="isLoading"
              class="rounded-lg border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 transition-all duration-200"
            >
              <template #prefix>
                <LockOutlined class="text-gray-400 text-base" />
              </template>
            </a-input-password>
          </a-form-item>

          <div class="flex justify-between items-center mb-6">
            <a-checkbox
              v-model:checked="form.rememberMe"
              class="text-sm text-gray-600"
            >
              Remember me
            </a-checkbox>
            <a
              href="#"
              class="text-sm text-blue-600 hover:text-blue-800 font-medium hover:underline transition-colors duration-200"
            >
              Forgot password?
            </a>
          </div>

          <a-form-item class="mb-5">
            <a-button
              type="primary"
              html-type="submit"
              size="large"
              block
              :loading="isLoading"
              :disabled="isLoading"
              class="h-12 bg-gradient-to-r from-blue-600 to-blue-700 hover:from-blue-700 hover:to-blue-800 border-none rounded-lg font-semibold text-base shadow-lg hover:shadow-xl transform hover:-translate-y-0.5 transition-all duration-200 disabled:transform-none"
            >
              <span v-if="!isLoading">Sign In</span>
            </a-button>
          </a-form-item>
        </a-form>

        <!-- Footer -->
        <div class="text-center pt-6 border-t border-gray-200">
          <p
            class="text-xs text-gray-500 flex items-center justify-center gap-2"
          >
            <ShieldCheckOutlined class="text-green-500" />
            Protected by enterprise-grade security
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";
import {
  UserOutlined,
  LockOutlined,
  // ShieldCheckOutlined,
} from "@ant-design/icons-vue";
import { login } from "@/stores/login.js";
import { message } from "ant-design-vue";

const form = ref({
  email: "",
  password: "",
});

const isLoading = ref(false);

const handleLogin = async () => {
  console.log(form.value);
  try {
    isLoading.value = true;
    const res = await login(form.value, $router);

    if (res) {
      message.success("Login successful!");
    }
  } catch (error) {
    message.error("Login failed. Please check your credentials.");
  } finally {
    isLoading.value = false;
  }
};
</script>

<style>
/* Override Ant Design styles with Tailwind-compatible approach */
.ant-input-affix-wrapper {
  @apply px-4 py-3;
}

.ant-input-affix-wrapper:focus,
.ant-input-affix-wrapper-focused {
  @apply ring-2 ring-blue-200 border-blue-500;
}

.ant-input-affix-wrapper:hover {
  @apply border-gray-400;
}

.ant-form-item-explain-error {
  @apply text-red-500 text-sm mt-1;
}

.ant-checkbox-wrapper {
  @apply text-gray-600;
}

.ant-btn-primary {
  @apply shadow-none;
}

.ant-btn-primary:hover {
  @apply shadow-none;
}
</style>
