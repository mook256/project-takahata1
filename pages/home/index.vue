<template>
    <div>
        <title>TAKAHATA</title>

        <!-- Box content with text -->
        <header class="box-content h-8 w-50 p-4 bg-Slate-50">
            <div class="flex items-center justify-between">
                <a href="#" class="-m-1.5 p-1.5">
                    <img class="h-8 w-auto" src="/image/ex.png" alt="" />
                </a>

             

                <div class="flex items-center space-x-2">


                    <div class="relative">
                        <!-- Dropdown Menu Button -->
                        <button @click="toggleDropdown" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Notification
                        </button>
                        <div v-if="dropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
                            <router-link to="/Alert_Request_User/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Request</a>
                            </router-link>
                            <router-link to="/Alert_Services_User/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Services</a>
                            </router-link>
                        </div>
                    </div>
                    <a href="https://sites.google.com/view/takahata" target="_blank">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Shuttle booking
                        </button>
                    </a>
                    <router-link to="/">
                        <button class="rounded-full bg-green-500 text-white px-4 py-2">
                            Logout
                        </button>
                    </router-link>
                    <!-- Dropdown Menu Button -->


                </div>
            </div>
        </header>
        <div>
            <main>
                <div class="p-4 bg-sky-300">
                    <h2 class="text-2xl   bg-white font-semibold rounded-lg  text-center mb-8 p-2 px-5"> Welcome {{ token.name }} </h2>
                    <div class="flex  justify-around ">
                        <div class="card bg-white rounded-lg p-4   ">
                            <img class=" " src="/image/t4.png" width="700" height="700" />
                        </div>
                    </div>
                </div>
            </main>

            <!-- Read Only Modal -->
            <div v-if="isViewing" class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center">
                <div class="bg-white p-4 rounded-lg">
                    <h3 class="font-semibold">Card Details</h3>
                    <img :src="viewData.image" alt="Preview" class="mb-4 max-w-full h-auto">
                    <p class="font-semibold">{{ viewData.title }}</p>
                    <p>{{ viewData.description }}</p>
                    <button @click="closeView()" class="bg-red-500 px-4 py-2 text-white rounded">Close</button>
                </div>
            </div>
        </div>

        <div class="mt-6">
            <!-- IT Request and IT Services -->
            <div class="grid grid-cols-2 gap-4">
                <!-- Column 1 -->
                <div class="border-gray-400 p-5 flex flex-col justify-center items-center">
                    <h2 class="text-lg font-semibold mb-4">IT Request(ระบบใบคำร้องขอ)</h2>

                    <!-- Content for column 1 -->
                    <router-link to="/Request/">
                        <button class="rounded-full bg-blue-500 text-white px-4 py-2">Click here</button>
                    </router-link>
                </div>

                <!-- Column 2 -->
                <div class="border-gray-400 p-2 flex flex-col justify-center items-center">
                    <h2 class="text-lg font-semibold mb-4">IT Services(ระบบแจ้งซ่อม)</h2>

                    <!-- Content for column 2 -->
                    <router-link to="/Services/">
                        <button class="rounded-full bg-blue-500 text-white px-4 py-2">
                            Click here
                        </button>
                    </router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { jwtDecode } from 'jwt-decode';

const token = jwtDecode(useCookie("token").value);
const cards = ref([
    { title: 'Card Title 1', description: 'Description of what card 1 is about.', image: '/image/ex.png' },
    { title: 'Card Title 2', description: 'Description of what card 2 is about.', image: '/image/ex.png' },
    { title: 'Card Title 3', description: 'Description of what card 3 is about.', image: '/image/ex.png' }
]);

const isViewing = ref(false);
const viewData = reactive({ title: '', description: '', image: '' });
const dropdownOpen = ref(false);

function viewCard(card) {
    viewData.title = card.title;
    viewData.description = card.description;
    viewData.image = card.image;
    isViewing.value = true;
}

function closeView() {
    isViewing.value = false;
}

function toggleDropdown() {
    dropdownOpen.value = !dropdownOpen.value;
}
</script>

<style scoped>
/* Existing styles */
</style>
