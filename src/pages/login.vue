<template>
  <div>
    <h1>Login Page</h1>
    <div class="flex flex-col gap-2">
      <input
        type="text"
        required
        v-model="username"
        class="border"
        placeholder="Username"
      />
      <input
        type="password"
        required
        v-model="password"
        class="border"
        placeholder="Password"
      />
      <button @click="onLogin()" class="bg-blue-500 text-white py-1 px-3">
        Login
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useAuthStore } from "@/stores/auth";
import axios from "axios";

const auth = useAuthStore();
const username = ref("");
const password = ref("");
const router = useRouter();

// Check if both username and password are not empty
const onLogin = async () => {
  try {
    const response = await axios.post("http://localhost:3000/login", {
      username: username.value,
      password: password.value,
    });

    if (response.data.status === "success") {
      auth.login(username.value);
      router.push("/");
    } else {
      window.alert("Username atau password salah");
    }
  } catch (error) {
    console.error("Error during login:", error);
    window.alert("Terjadi kesalahan saat melakukan login");
  }
};
</script>