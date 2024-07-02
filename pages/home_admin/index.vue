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
                        <button @click="toggleDropdown1" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Menu
                        </button>
                        <div v-if="dropdownOpen1" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
                            <router-link to="/appl_yuser/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Applyuser</a>
                            </router-link>
                            <router-link to="/UserDataTable/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">User Data</a>
                            </router-link>
                        </div>
                    </div>
                    <div class="relative">
                        <!-- Dropdown Menu Button -->
                        <button @click="toggleDropdown" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Notification
                        </button>
                        <div v-if="dropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
                            <router-link to="/Request_admin/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Request</a>
                            </router-link>
                            <router-link to="/Services_admin/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Services</a>
                            </router-link>
                            <router-link to="/Report/">
                                <a @click="clearSavedDate" class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Report</a>
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
                    <h2 class="text-2xl   bg-white font-semibold rounded-lg  text-center mb-8 p-2 px-5"> Welcome {{
                        token.name }} </h2>
                    <div class="flex  justify-around ">
                        <div class="card bg-white rounded-lg p-4   ">
                            <img class=" " src="/image/t4.png" width="700" height="700" />
                        </div>
                    </div>
                </div>
            </main>

            <!-- Edit Modal -->
            <div v-if="isEditing" class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center">
                <div class="bg-white p-4 rounded-lg">
                    <h3 class="font-semibold">Edit Card</h3>
                    <input type="file" @change="handleFileUpload" class="mb-2">
                    <img :src="editData.image" alt="Preview" class="mb-4 max-w-full h-auto">
                    <input v-model="editData.title" class="border p-2 w-full mb-2">
                    <textarea v-model="editData.description" class="border p-2 w-full mb-2"></textarea>
                    <button @click="saveEdit()" class="bg-green-500 px-4 py-2 text-white rounded">Save</button>
                    <button @click="cancelEdit()" class="bg-red-500 px-4 py-2 text-white rounded">Cancel</button>
                </div>
            </div>
        </div>

    </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { jwtDecode } from 'jwt-decode';

const token = jwtDecode(useCookie("token").value);


const isEditing = ref(false);
const editData = reactive({ title: '', description: '', image: '' });
const dropdownOpen = ref(false);
const dropdownOpen1 = ref(false);


function handleFileUpload(event) {
    const reader = new FileReader();
    reader.onload = (e) => {
        editData.image = e.target.result;
    };
    reader.readAsDataURL(event.target.files[0]);
}


function toggleDropdown() {
    dropdownOpen.value = !dropdownOpen.value;
}

function toggleDropdown1() {
    dropdownOpen1.value = !dropdownOpen1.value;
}

const clearSavedDate = () => {
    localStorage.removeItem('selectedDateStart');
    sessionStorage.removeItem('selectedDateStart');

    localStorage.removeItem('selectedDateEnd');
    sessionStorage.removeItem('selectedDateEnd');

    localStorage.removeItem('selectedTopic');
    sessionStorage.removeItem('selectedTopic');

    localStorage.removeItem('selectedIssue');
    sessionStorage.removeItem('selectedIssue');

    localStorage.removeItem('reTopic');
    sessionStorage.removeItem('reTopic');

    localStorage.removeItem('reIssue');
    sessionStorage.removeItem('reIssue');
    window.location.href = '/Report'; // Redirect to the calendar page
  };
</script>

<style scoped>
/* Existing styles */
</style>