<template>
  <div
    class="min-h-screen w-full flex items-center justify-center p-6 bg-gray-50"
  >
    <!-- Login Card -->
    <div
      class="w-full max-w-md bg-white rounded-2xl shadow-lg border border-gray-200 p-8 transition-all duration-300"
    >
      <!-- Brand -->
      <div class="text-center mb-8">
        <div
          class="inline-flex items-center justify-center w-14 h-14 rounded-xl bg-gray-800 shadow-md mb-4"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="28"
            height="28"
            viewBox="0 0 24 24"
            fill="none"
            stroke="white"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z" />
          </svg>
        </div>
        <h1 class="text-3xl font-bold text-gray-800">LynxxSpace</h1>
        <p class="text-sm text-gray-500 mt-1">
          Temukan Ketenangan dalam Produktivitas
        </p>
      </div>

      <!-- Tab Selector -->
      <div class="flex gap-1 p-1 bg-gray-100 rounded-xl mb-6">
        <button
          @click="isLoginMode = true"
          :class="[
            'flex-1 py-2.5 text-sm font-medium rounded-lg transition-all duration-200',
            isLoginMode
              ? 'bg-white text-gray-900 shadow-sm border border-gray-200'
              : 'text-gray-500 hover:text-gray-700',
          ]"
        >
          Masuk
        </button>
        <button
          @click="isLoginMode = false"
          :class="[
            'flex-1 py-2.5 text-sm font-medium rounded-lg transition-all duration-200',
            !isLoginMode
              ? 'bg-white text-gray-900 shadow-sm border border-gray-200'
              : 'text-gray-500 hover:text-gray-700',
          ]"
        >
          Daftar
        </button>
      </div>

      <!-- Alert -->
      <transition name="slide-fade">
        <div v-if="alert.message" class="mb-6">
          <div
            :class="[
              'flex items-center gap-3 px-4 py-3 rounded-lg text-sm',
              alert.type === 'success'
                ? 'bg-green-50 text-green-800 border border-green-200'
                : 'bg-red-50 text-red-800 border border-red-200',
            ]"
          >
            <span class="flex-shrink-0">
              <svg
                v-if="alert.type === 'success'"
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M20 6 9 17l-5-5" />
              </svg>
              <svg
                v-else
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <circle cx="12" cy="12" r="10" />
                <path d="m15 9-6 6" />
                <path d="m9 9 6 6" />
              </svg>
            </span>
            <p class="flex-1">{{ alert.message }}</p>
          </div>
        </div>
      </transition>

      <!-- Form -->
      <form @submit.prevent="handleSubmit" class="space-y-5">
        <!-- Name (Register Only) -->
        <transition name="expand">
          <div v-if="!isLoginMode" class="space-y-2">
            <label class="block text-sm font-medium text-gray-700"
              >Nama Lengkap</label
            >
            <div class="relative">
              <svg
                class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2" />
                <circle cx="12" cy="7" r="4" />
              </svg>
              <input
                type="text"
                v-model="formData.name"
                class="w-full pl-10 pr-4 py-2.5 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-400 focus:border-transparent bg-white text-gray-900 transition-all duration-200"
                placeholder="Masukkan nama lengkap"
                required
              />
            </div>
          </div>
        </transition>

        <!-- Email -->
        <div class="space-y-2">
          <label class="block text-sm font-medium text-gray-700"
            >Alamat Email</label
          >
          <div class="relative">
            <svg
              class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <rect width="20" height="16" x="2" y="4" rx="2" />
              <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7" />
            </svg>
            <input
              type="email"
              v-model="formData.email"
              class="w-full pl-10 pr-4 py-2.5 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-400 focus:border-transparent bg-white text-gray-900 transition-all duration-200"
              placeholder="nama@email.com"
              required
            />
          </div>
        </div>

        <!-- Password -->
        <div class="space-y-2">
          <label class="block text-sm font-medium text-gray-700"
            >Kata Sandi</label
          >
          <div class="relative">
            <svg
              class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <rect width="18" height="11" x="3" y="11" rx="2" ry="2" />
              <path d="M7 11V7a5 5 0 0 1 10 0v4" />
            </svg>
            <input
              :type="showPassword ? 'text' : 'password'"
              v-model="formData.password"
              class="w-full pl-10 pr-12 py-2.5 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-400 focus:border-transparent bg-white text-gray-900 transition-all duration-200"
              placeholder="••••••••"
              required
            />
            <button
              type="button"
              @click="showPassword = !showPassword"
              class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600 transition-colors"
            >
              <svg
                v-if="showPassword"
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path
                  d="M2.062 12.348a1 1 0 0 1 0-.696 10.75 10.75 0 0 1 19.876 0 1 1 0 0 1 0 .696 10.75 10.75 0 0 1-19.876 0Z"
                />
                <circle cx="12" cy="12" r="3" />
              </svg>
              <svg
                v-else
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M9.88 9.88a3 3 0 1 0 4.24 4.24" />
                <path
                  d="M10.73 5.08A10.43 10.43 0 0 1 12 5c7 0 10 7 10 7a13.16 13.16 0 0 1-1.67 2.68"
                />
                <path
                  d="M6.61 6.61A13.52 13.52 0 0 0 2 12s3 7 10 7a9.74 9.74 0 0 0 5.39-1.61"
                />
                <line x1="2" x2="22" y1="2" y2="22" />
              </svg>
            </button>
          </div>
        </div>

        <!-- Submit Button -->
        <button
          type="submit"
          :disabled="isLoading"
          class="w-full py-2.5 bg-gray-800 hover:bg-gray-700 text-white font-medium rounded-lg transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed shadow-sm"
        >
          <span v-if="isLoading" class="inline-flex items-center gap-2">
            <svg
              class="animate-spin h-4 w-4"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
            >
              <circle
                class="opacity-25"
                cx="12"
                cy="12"
                r="10"
                stroke="currentColor"
                stroke-width="4"
              ></circle>
              <path
                class="opacity-75"
                fill="currentColor"
                d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
              ></path>
            </svg>
            Memproses...
          </span>
          <span v-else>{{
            isLoginMode ? "Masuk ke LynxxSpace" : "Mulai Registrasi"
          }}</span>
        </button>
      </form>

      <!-- Footer Note -->
      <p class="text-center text-xs text-gray-500 mt-6">
        © 2024 LynxxSpace. All rights reserved.
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const isLoginMode = ref(true);
const showPassword = ref(false);
const isLoading = ref(false);

const formData = reactive({
  name: "",
  email: "",
  password: "",
});

const alert = reactive({
  message: "",
  type: "",
});

watch(isLoginMode, () => {
  alert.message = "";
  alert.type = "";
  formData.name = "";
  formData.email = "";
  formData.password = "";
});

const showAlert = (message, type = "error") => {
  alert.message = message;
  alert.type = type;

  if (type === "success") {
    setTimeout(() => {
      alert.message = "";
    }, 4000);
  }
};

const handleSubmit = async () => {
  alert.message = "";
  isLoading.value = true;

  const endpoint = isLoginMode.value
    ? "https://backendrepo-production-2f75.up.railway.app/api/auth/login"
    : "https://backendrepo-production-2f75.up.railway.app/api/auth/register";

  const payload = isLoginMode.value
    ? { email: formData.email, password: formData.password }
    : {
        name: formData.name,
        email: formData.email,
        password: formData.password,
      };

  console.log("Sending request to:", endpoint);
  console.log("Payload:", payload);

  try {
    const response = await fetch(endpoint, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
      body: JSON.stringify(payload),
    });

    console.log("Response status:", response.status);

    const data = await response.json();
    console.log("Response data:", data);

    if (!response.ok) {
      throw new Error(data.message || "Terjadi kesalahan sistem.");
    }

    showAlert(data.message, "success");

    // Simpan dengan nama LynxxSpace
    localStorage.setItem("LynxxSpace_token", data.token);
    localStorage.setItem("LynxxSpace_user", JSON.stringify(data.user));

    setTimeout(() => {
      router.push("/dashboard");
    }, 1000);
  } catch (error) {
    console.error("Fetch error details:", error);
    showAlert(error.message);
  } finally {
    isLoading.value = false;
  }
};
</script>

<style scoped>
/* Transitions */
.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all 0.3s ease;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}

.expand-enter-active,
.expand-leave-active {
  transition: all 0.3s ease;
  max-height: 100px;
  opacity: 1;
}

.expand-enter-from,
.expand-leave-to {
  max-height: 0;
  opacity: 0;
  margin-bottom: 0;
  transform: translateY(-5px);
}

/* Custom focus styles */
input:focus {
  outline: none;
}

/* Smooth hover effects */
button {
  cursor: pointer;
}

/* Disabled button state */
button:disabled {
  cursor: not-allowed;
}
</style>
