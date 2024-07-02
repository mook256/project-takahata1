<template>
  <title>TAKAHATA</title>
  <div class="flex min-h-full flex-1 flex-col justify-center px-6 py-12 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-sm">
      <div class="bg-indigo-30">
        <img class="object-cover" src="/image/ex1.jpeg" h-10 w-10 />
      </div>
      <h2 class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900"> SERVICESDESK-LOGIN</h2>
    </div>

    <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
      <div>
        <label for="Username" class="block text-sm font-medium leading-6 text-gray-900">Username</label>
        <div class="">
          <input v-model="user" id="Username" name="Username" type="text" autocomplete="username" required
            class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
        </div>
      </div>

      <div class="mt-3">
        <div class="flex items-center justify-between">
          <label for="password" class="block text-sm font-medium leading-6 text-gray-900">Password</label>
        </div>
        <div class="">
          <input v-model="password" id="password" name="password" type="password" autocomplete="current-password"
            required
            class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
        </div>
      </div>

      <div class="mt-5">
        <h1 class="text-red-500">{{ reason }}</h1>
        <button @click="login"
          class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
          Login
        </button>
      </div>
    </div>
    <h1 class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900">Copyright 2024 Rev1.0 By
      Kunanya_ks. </h1>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { jwtDecode } from "jwt-decode";

const user = ref('')
const password = ref('')
const reason = ref('')



const login = async () => {
  const res = await $fetch('http://172.29.10.238:8080/api/login', {
    method: "POST",
    body: JSON.stringify({
      user: user.value,
      password: password.value
    }),
    credentials: 'include',
    headers: {
      'Content-Type': 'application/json'
    }
  }).then(() => {
    const token = jwtDecode(useCookie("token").value);
    // Handle successful login 
    //console.log(token.role)
    if (token.role == '0') {
      navigateTo('/home_admin')
    }
    else if (token.role == '1') {
      navigateTo('/home')
    }
    else {
      navigateTo('/home_manager')
    }

  }).catch(error => {
    // Handle login failure
    reason.value = error.data.message
  })
}
</script>