<template>
  <div class="flex justify-center items-center min-h-screen p-4">
    <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-sm">
      <h1 class="text-3xl font-semibold text-center text-emerald-900 mb-6">
        Login
      </h1>

      <!-- Display error message -->
      <div v-if="error" class="bg-red-500 text-white text-sm p-2 mb-4 rounded">
        {{ error }}
      </div>

      <form @submit.prevent="login" class="shadow shadow-xl">
        <div class="mb-4">
          <label
            for="username"
            class="block text-sm font-medium text-emerald-900 mb-2"
            >Username</label
          >
           <UInput 
            v-model="form.username"
            required
            size="md"
            placeholder="Enter your username">
            <template #trailing>
                    <UButton
                    color="gray"
                    variant="link"
                    icon="i-heroicons-user"
                    :padded="false"
                />
            </template>
          </UInput>
        </div>
        
        <!-- Password field with eye icon to toggle visibility -->
        <div class="mb-6 relative">
          <label
            for="password"
            class="block text-sm font-medium text-emerald-900 mb-2"
            >Password</label
          >
          <!-- <input
            :type="showPassword ? 'text' : 'password'"
            v-model="form.password"
            required
            class="w-full p-2 border border-emerald-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-yellow-500"
            placeholder="Enter your password"
          /> -->
          <UInput 
            :type="showPassword ? 'text' : 'password'"
            v-model="form.password"
            required
            size="md"
            :ui="{ icon: { trailing: { pointer: '' } } }"
            placeholder="Enter your password">
                <template #trailing>
                     <UButton
                        color="gray"
                        :icon="showPassword ? 'i-heroicons-eye-slash' : 'i-heroicons-eye'"
                        :padded="false"
                        @click.prevent="showPassword = !showPassword"
                    />
                </template>
          </UInput>
          <!-- Eye icon to toggle password visibility -->
          <span
            @click="togglePasswordVisibility"
            class="absolute right-3 top-1/2 transform cursor-pointer"
          >
            <font-awesome :icon="showPassword ? faEye : faEyeSlash" class="text-emerald-900 text-xl" />
          </span>
        </div>

        <UButton color="yellow" type="submit" width="w-full" size="md" class="flex w-full items-center justify-center" variant="solid">Login</UButton>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios"; // Import axios for API requests
import { useAuthStore } from "@/store/auth"; // Import the auth store
import { useRouter } from "vue-router"; // Import the router for navigation
import Swal from "sweetalert2"; // Import SweetAlert2
import { FormDx, handleApiError } from "@/utility/Helper.js";
import { VUE_APP_API_URL } from "@/utility/Global.js";
// Define reactive variables
const error = ref(""); // Declare error ref to store the error message
const router = useRouter();

const form = ref({
  username: "",
  password: "",
});

const showPassword = ref(false); // Boolean to toggle password visibility

// Toggle function to show/hide password
const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

// Login function using axios for HTTP request
const login = async () => {
  // Show the loading spinner alert using SweetAlert2
  const swalInstance = Swal.fire({
    title: 'Logging in...',
    text: 'Please wait while we authenticate you.',
    allowOutsideClick: false,  // Prevent closing the modal by clicking outside
    didOpen: () => {
      Swal.showLoading();  // Show the loading spinner
    }
  });

  try {
    const formData = FormDx(form.value);
    const response = await axios.post(`${VUE_APP_API_URL}auth/login`, formData);
    if (response.data && response.data.accessToken){
        // Assuming the response contains a token and user data
        const { accessToken } = response.data;

        console.log(response.data);
        // Save token and user data to store
        const authStore = useAuthStore();
        authStore.setAuthData(accessToken, response.data); // Store token and set authenticated to true

        // Close the Swal loading spinner
        swalInstance.close();

        router.push("/admin/dashboard"); // Redirect to the homepage or dashboard
    }
  } catch (error) {
    // Close the Swal loading spinner
    swalInstance.close();
    console.log(error);
    // Handle API error, show error message
    handleApiError(error);  // You can display this via Swal or console
  }
};
</script>
