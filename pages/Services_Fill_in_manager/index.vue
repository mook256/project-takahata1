<template>
    <div>
        <!-- Header -->
        <header class="box-content h-8 w-50 p-4 bg-Slate-50">
            <div class="flex items-center justify-between">
                <a href="#" class="-m-1.5 p-1.5">
                    <img class="h-8 w-auto" src="/image/ex.png" alt="" />
                </a>
                <div class="flex items-center space-x-2">
                    <router-link to="/home_manager/">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Home</button>
                    </router-link>
                    <a href="https://sites.google.com/view/takahata" target="_blank">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Shuttle booking</button>
                    </a>
                    <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Notification</button>
                    <router-link to="/">
                        <button class="rounded-full bg-green-500 text-white px-4 py-2">Logout</button>
                    </router-link>
                </div>
            </div>
        </header>

        <!-- Main Content -->
        <div class="bg-sky-300 ">
            <main class="container mx-auto py-10 bg-Teal-600">
                <div class="text-center mb-8">
                    <h2 class="text-2xl font-semibold">ระบบแจ้งซ่อม</h2>
                </div>
                <div class="container box-border mx-auto max-w-prose bg-sky-50 ">

                    <div class="mt-10">
                        <!-- Form -->
                        <form class="mx-auto max-w-lg" @submit.prevent="validateForm">
                            <div class="mb-4">
                                <label for="Service type" class="block mb-2 font-bold">Service type </label>
                                <select name="Service type" id="Service type"
                                    class="h-full rounded-md border-0 bg-transparent py-0 pl-2 pr-7 text-gray-900 sm:text-sm outline outline-offset-2 outline-white-500"
                                    v-model="selectedTopic" @change="loadIssues" required>
                                    <option v-for="i in ty1" :key="i.id" :value="i.name">{{ i.name }}</option>
                                </select>
                            </div>

                            <!-- Select Issue -->
                            <div class="mb-4" v-if="ty2.length > 0">
                                <label for="Catalog type" class="block mb-2 font-bold">Catalog type</label>
                                <select name="Catalog type" id="Catalog type"
                                    class="h-full rounded-md border-0 bg-transparent py-0 pl-2 pr-7 text-gray-900 sm:text-sm outline outline-offset-2 outline-white-500"
                                    v-model="selectedIssue" @change="loadLevel" required>
                                    <option v-for="j in ty2" :key="j.id">{{ j.name }}</option>
                                </select>
                            </div>

                            <div class="mb-4" v-if="ty3.length > 0">
                                <label for="Level" class="block mb-2 font-bold">Level</label>
                                <select name="issue" id="Level"
                                    class="h-full rounded-md border-0 bg-transparent py-0 pl-2 pr-7 text-gray-900 sm:text-sm outline outline-offset-2 outline-white-500"
                                    v-model="selectedLevel" required>
                                    <option v-for="l in ty3" :key="l.id">{{ l.name }}</option>
                                </select>
                            </div>

                            <!-- detail -->
                            <div class="mb-4">
                                <label for="detail" class="block mb-2 font-bold">Description</label>
                                <textarea id="detail"
                                    class="form-control h-32 w-full border rounded-lg px-4 outline outline-offset-2 outline-white-500"
                                    rows="3" v-model="detail" required></textarea>
                                <p v-if="errorMessage" class="text-red-600 mt-2">{{ errorMessage }}</p>
                            </div>
                        </form>
                        <!-- Buttons -->
                        <div class="flex items-center justify-center space-x-2 flex-grow">
                            <button @click="showModal = true"
                                class="rounded-full bg-green-500 text-white px-4 py-2">Submit
                            </button>
                            <router-link to="/home/">
                                <button class="rounded-full bg-red-600 text-white px-4 py-2">Cancel</button>
                            </router-link>

                        </div>

                        <!-- Modal -->
                        <div v-if="showModal"
                            class="fixed inset-0 flex items-center justify-center z-50 bg-gray-800 bg-opacity-75">
                            <div class="bg-white p-6 rounded-lg shadow-lg">
                                <h3 class="text-lg font-semibold mb-4">Confirm Submission</h3>
                                <p class="mb-4">Are you sure you want to submit this form?</p>
                                <div class="flex justify-end space-x-2">
                                    <button @click="handleSubmit"
                                        class="rounded-full bg-green-500 text-white px-4 py-2">YES
                                    </button>

                                    <button @click="showModal = false"
                                        class="rounded-full bg-red-600 text-white px-4 py-2">Cancel
                                    </button>
                                </div>
                            </div>
                        </div>

                        <!-- Buttons -->
                        <div class="mt-6">
                            <div class="flex items-center justify-center space-x-2">

                            </div>
                        </div>

                    </div>
                </div>
            </main>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { jwtDecode } from "jwt-decode";
import { useRouter } from 'vue-router';

const ty1 = ref([]);
const ty2 = ref([]);
const ty3 = ref({});
const selectedTopic = ref();
const selectedIssue = ref();
const selectedLevel = ref();
const detail = ref('');
const showModal = ref(false);
const errorMessage = ref('');

const token = jwtDecode(useCookie("token").value); // nuxt cookie & jwtDecode nuxt

const fetchData = async () => {
    try {
        const response = await $fetch('http://172.29.10.238:8080/api/get/typeRepair', { credentials: 'include' });
        ty1.value = response.results;
    } catch (error) {
        console.error('Failed to fetch Topic type data:', error);
    }
};

const loadIssues = async () => {
    try {
        if (selectedTopic.value) {
            const response = await $fetch('http://172.29.10.238:8080/api/get/resource', {
                method: "POST",
                body: { type: selectedTopic.value },
                credentials: 'include'
            });
            ty2.value = response.results;
        }
    } catch (error) {
        console.error('Failed to fetch ty2 data:', error);
    }
};

const loadLevel = async () => {
    try {
        if (selectedTopic.value) {
            const response = await $fetch('http://172.29.10.238:8080/api/Level_ServiceController', {
                method: "POST",
                credentials: 'include'
            });
            ty3.value = response.results;
        }
    } catch (error) {
        console.error('Failed to fetch ty3 data:', error);
    }
};

const saveData = async () => {
    try {
        const response = await $fetch('http://172.29.10.238:8080/api/IT_Service_data', {
            method: "POST",
            body: {
                en: token.userId,
                name: token.name,
                section_id: token.department,
                branch: token.branch,
                tel: token.tel,
                service_type: selectedTopic.value,
                catalog_type: selectedIssue.value,
                detail: detail.value,
                status: "Open",
                level: selectedLevel.value,
            },
            credentials: 'include'
        });
        if (response.ok) {
            navigateTo('/home/');
        } else {
            console.error('Submission failed:', response.statusText);
        }
    } catch (error) {
        console.error('Failed to save data:', error);
    }
};

const validateForm = () => {
    errorMessage.value = '';  // Reset error message

    if (!detail.value) {
        errorMessage.value = 'Explanation is required';
        return false;
    }

    showModal.value = true;
    return true;
};

const handleSubmit = async () => {
    if (validateForm()) {
        await saveData();  // Submit the form data
        showModal.value = false;  // Close the modal
    }
};

fetchData();
</script>

<style scoped>
/* Add any scoped styles here */
</style>