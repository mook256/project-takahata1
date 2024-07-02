<template>
    <div>
        <title>TAKAHATA</title>
        <header class="box-content h-8 w-50 p-4 bg-slate-50">
            <div class="flex items-center justify-between">
                <a href="#" class="-m-1.5 p-1.5">
                    <img class="h-8 w-auto" src="/image/ex.png" alt="" />
                </a>
                <div class="text-center mt-4">
                    <!-- Display user name from token -->
                    <p class="text-lg font-semibold" style="margin-left: 100px;">
                        {{ token.name }}
                    </p>
                </div>
                <div class="flex items-center space-x-2">
                    <a href="https://sites.google.com/view/takahata" target="_blank">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Shuttle booking
                        </button>
                    </a>
                    <router-link to="/Notifications/">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Notification
                        </button>
                    </router-link>
                    <router-link to="/">
                        <button class="rounded-full bg-green-500 text-white px-4 py-2">
                            Logout
                        </button>
                    </router-link>
                </div>
            </div>
        </header>
        <main>
            <div class="box-content h-8 w-50 p-4 bg-sky-500">

                <h2 class="text-2xl font-semibold text-center mb-8">Official Activities</h2>
                <div class="flex justify-around space-x-6 mx-auto w-full">
                    <div class="card bg-white rounded-lg shadow-md p-4 max-w-sm">
                        <h3 class="text-lg font-semibold text-center mb-4">Card Title 1</h3>
                        <p class="text-center">Description of what card 1 is about.</p>
                        <span class="block text-sm font-medium text-slate-700">Username</span>
                        <p class="text-sm font-medium text-slate-900">{token.name}</p>
                        <p class="text-sm text-slate-500 truncate">{token.email}</p>
                    </div>


                </div>

            </div>
        </main>
        <div>

        </div>
    </div>
</template>

<script setup>
const name = ref();
const email = ref();
import { ref } from 'vue';
import { jwtDecode } from 'jwt-decode';
const token = jwtDecode(useCookie("token").value);
</script>

<style scoped>
.text-lg {
    text-align: center;
    margin: 0 auto;
}
</style>