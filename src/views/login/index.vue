<template>
  <section
    class="min-h-screen bg-gradient-to-br from-blue-900 via-purple-900 to-indigo-900 flex items-center justify-center p-4"
  >
    <div class="w-full max-w-md">
      <!-- Logo/Image Section -->
      <div class="text-center mb-8">
        <div
          class="w-20 h-20 bg-white/10 backdrop-blur-sm rounded-full flex items-center justify-center mx-auto mb-4"
        >
          <img
            src="@/assets/images/login.png"
            class="w-12 h-12 object-contain"
            alt="Login"
          />
        </div>
        <h1 class="text-2xl font-bold text-white mb-2">Welcome Back</h1>
        <p class="text-white/70 text-sm">Please sign in to your account</p>
      </div>

      <!-- Login Form Card -->
      <div
        class="bg-white/10 backdrop-blur-md rounded-2xl p-8 shadow-2xl border border-white/20"
      >
        <a-form :model="form" @finish="handleLogin" class="space-y-6">
          <!-- Email Field -->
          <a-form-item
            name="email"
            :rules="[{ required: true, message: 'Please input your email!' }]"
            class="mb-0"
          >
            <div class="relative">
              <div
                class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
              >
                <svg
                  class="w-5 h-5 text-white/50"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207"
                  />
                </svg>
              </div>
              <a-input
                placeholder="Email address"
                class="w-full pl-10 pr-4 py-3 bg-white/5 border border-white/20 rounded-lg text-white placeholder-white/50 focus:border-white/40 focus:ring-2 focus:ring-white/20 transition-all duration-200"
                v-model:value="form.email"
              />
            </div>
          </a-form-item>

          <!-- Password Field -->
          <a-form-item
            name="password"
            :rules="[
              { required: true, message: 'Please input your password!' },
            ]"
            class="mb-0"
          >
            <div class="relative">
              <div
                class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
              >
                <svg
                  class="w-5 h-5 text-white/50"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"
                  />
                </svg>
              </div>
              <a-input-password
                placeholder="Password"
                class="w-full pl-10 pr-4 py-3 bg-white/5 border border-white/20 rounded-lg text-white placeholder-white/50 focus:border-white/40 focus:ring-2 focus:ring-white/20 transition-all duration-200"
                v-model:value="form.password"
              />
            </div>
          </a-form-item>

          <!-- Remember Me & Forgot Password -->
          <div class="flex items-center justify-between text-sm">
            <label class="flex items-center text-white/70 cursor-pointer">
              <input
                type="checkbox"
                class="w-4 h-4 text-blue-600 bg-white/10 border-white/20 rounded focus:ring-blue-500 focus:ring-2"
              />
              <span class="ml-2">Remember me</span>
            </label>
            <a
              href="#"
              class="text-white/70 hover:text-white transition-colors duration-200"
            >
              Forgot password?
            </a>
          </div>

          <!-- Submit Button -->
          <a-form-item class="mb-0">
            <a-button
              html-type="submit"
              class="w-full bg-gradient-to-r from-blue-500 to-purple-600 hover:from-blue-600 hover:to-purple-700 text-white font-semibold py-3 px-4 rounded-lg transition-all duration-200 transform hover:scale-[1.02] focus:ring-4 focus:ring-blue-500/25 border-0"
              :disabled="isLoading"
            >
              <div class="flex items-center justify-center">
                <a-spin v-if="isLoading" class="mr-2" />
                <svg
                  v-if="!isLoading"
                  class="w-5 h-5 mr-2"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M11 16l-4-4m0 0l4-4m-4 4h14m-5 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h7a3 3 0 013 3v1"
                  />
                </svg>
                {{ isLoading ? "Signing in..." : "Sign In" }}
              </div>
            </a-button>
          </a-form-item>
        </a-form>

        <!-- Sign Up Link -->
        <div class="mt-6 text-center">
          <span class="text-white/70 text-sm">Don't have an account? </span>
          <a
            href="#"
            class="text-white font-medium hover:text-blue-300 transition-colors duration-200"
          >
            Sign up
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";
import { login } from "@/stores/login.js";
import { useRouter } from "vue-router";

const form = ref({
  email: "",
  password: "",
});
const isLoading = ref(false);

const router = useRouter();

const handleLogin = async () => {
  isLoading.value = true;
  try {
    const res = await login(form.value, router);
    if (res) {
      isLoading.value = false;
    }
  } catch (error) {
    isLoading.value = false;
    console.error("Login error:", error);
  }
};
</script>

<style lang="scss">
// Custom styles for Ant Design components to work with the glass morphism design
.ant-input,
.ant-input-password .ant-input {
  background: transparent !important;
  border: 1px solid rgba(255, 255, 255, 0.2) !important;
  color: white !important;

  &:hover,
  &:focus {
    border-color: rgba(255, 255, 255, 0.4) !important;
    box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.1) !important;
  }

  &::placeholder {
    color: rgba(255, 255, 255, 0.5) !important;
  }
}

.ant-input-password-icon {
  color: rgba(255, 255, 255, 0.5) !important;

  &:hover {
    color: rgba(255, 255, 255, 0.8) !important;
  }
}

.ant-form-item-explain-error {
  color: #fca5a5 !important;
}

.ant-btn[disabled] {
  opacity: 0.6 !important;
  cursor: not-allowed !important;
}

.ant-spin-dot-item {
  background-color: white !important;
}
</style>
