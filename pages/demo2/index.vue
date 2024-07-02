<template>
    <div>
        <!-- Header -->
        <header class="box-content h-8 w-50 p-4 bg-Slate-50">
            <div class="flex items-center justify-between">
                <a href="#" class="-m-1.5 p-1.5">
                    <img class="h-8 w-auto" src="/image/ex.png" alt="" />
                </a>
                <div class="flex items-center space-x-2">
                    <router-link to="/home/">
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
                    <h2 class="text-2xl font-semibold">Report {{ reTopic }} {{ reIssue }}</h2>
                </div>
                <div class="container box-border mx-auto max-w-prose bg-sky-50 ">

                    <div class="mt-10">
                        <!-- Form -->
                        <form class="  mx-auto max-w-lg" @submit.prevent="validateForm">
                            <div class="mb-4 mt-3">
                                <label for="Service type" class="block mb-2 font-bold">Report by </label>
                                <select name="Service type" id="Service type"
                                    class="h-full rounded-md border-0 bg-transparent py-0 pl-2 pr-7 text-gray-900 sm:text-sm outline outline-offset-2 outline-white-500"
                                    v-model="selectedTopic" @change="loadGroupby" required>
                                    <option>IT-Service</option>
                                    <option>IT-Request</option>
                                    <option>Select All</option>
                                </select>
                            </div>

                            <!-- Select Group by -->
                            <div class="mb-4" v-if="selectedTopic">
                                <label for="Catalog type" class="block mb-2 font-bold">Group by</label>
                                <select name="Catalog type" id="Catalog type"
                                    class="h-full rounded-md border-0 bg-transparent py-0 pl-2 pr-7 text-gray-900 sm:text-sm outline outline-offset-2 outline-white-500"
                                    v-model="selectedIssue" required>
                                    <option v-for="j in ty2" :key="j.id">{{ j.topic }}</option>
             
                                </select>
                            </div>

                            <!-- วันที่ -->
                            <div class="date-wrapper" > <!--v-if="selectedIssue"-->
                                <div class="date-container">
                                    <label for="date">Start Date:</label>
                                    <input type="date" v-model="selectedDateStart">
                                </div>
                                <div class="date-container">
                                    <label for="date1">End Date</label>
                                    <input type="date" v-model="selectedDateEnd">
                                    <!--<button @click="refreshPage">Refresh test</button>-->
                                </div>
                            </div><button @click="refreshPage">Refresh test</button>


                            <!-- Buttons -->
                            <div class="mt-6">
                                <div class="flex items-center justify-center space-x-2">

                                </div>
                            </div>

                        </form>
                        <!-- Buttons -->
                        <div class="flex items-center justify-center space-x-2 flex-grow">
                            <button @click="showModal = true"
                                class="rounded-full bg-green-500 text-white px-4 py-2">Search</button>
                            <router-link to="/home/">
                                <button class="rounded-full bg-red-600 text-white px-4 py-2">Cancel</button>
                            </router-link>

                        </div>

                        <!-- Modal -->


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
import { onMounted, watch } from 'vue';

const ty2 = ref();

const selectedTopic = ref();
const selectedIssue = ref();
const reTopic = ref();
const reIssue = ref();
const selectedLevel = ref();
const detail = ref();
const selectedDateStart = ref('');
const selectedDateEnd = ref('');


onMounted(() => {

    const savedDateStart = localStorage.getItem('selectedDateStart');
    const savedDateEnd = localStorage.getItem('selectedDateEnd');
    const savedTopic2 = localStorage.getItem('reTopic');
    const savedIssue2 = localStorage.getItem('reIssue');

    if (savedDateStart && savedDateEnd && savedTopic2 && savedIssue2) {
        selectedDateStart.value = savedDateStart;
        selectedDateEnd.value = savedDateEnd;
        reTopic.value = savedTopic2;
        reIssue.value = savedIssue2;
        
        console.log(reTopic.value);
        console.log(reIssue.value);
    }

    else {
        // If the page is newly opened, clear sessionStorage
        sessionStorage.removeItem('selectedDateStart');
        sessionStorage.removeItem('selectedDateEnd');
        sessionStorage.removeItem('reTopic');
        sessionStorage.removeItem('reIssue');

    }
});

function watcher(key, date) {
    watch(date, (newDate) => {
        if (newDate) {
            localStorage.setItem(key, newDate);
        } else {
            localStorage.removeItem(key);
        }
    });
}

watcher('selectedDateStart', selectedDateStart);
watcher('selectedDateEnd', selectedDateEnd);
watcher('reTopic', selectedTopic);
watcher('reIssue', selectedIssue);


const refreshPage = () => {
    window.location.reload();
};

const token = jwtDecode(useCookie("token").value); // nuxt cookie & jwtDecode nuxt

const loadGroupby = async () => {
    try {
        selectedIssue.value = null;
        if (selectedTopic.value) {
            const response = await $fetch('http://172.29.10.238:8080/api/Report_GroupbyController', {
                method: "POST",
                body: {
                    topic: selectedTopic.value
                },
                credentials: 'include'
            });
            ty2.value = response.results;
            console.log(selectedTopic.value);
        }
    } catch (error) {
        console.error('Failed to fetch ty2 data:', error);
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
                branch: token.branch, //
                tel: token.tel,
                service_type: selectedTopic.value,
                catalog_type: selectedIssue.value,
                detail: detail.value,
                status: "Open",
                level: selectedLevel.value,
            },
            credentials: 'include'
        });
        if (response.ok) { // Check if the request was successful
            navigateTo('/home/');
        } else {
            console.error('Submission failed:', response.statusText);
            // Handle failure (maybe show a message to the user)
        }
    } catch (error) {
        console.error('Failed to save data:', error);
        // Handle errors (inform the user)
    }
};

const validateForm = () => {
    if (!detail.value) {
        return false;
    }
    showModal.value = true;
    return true;
};

const handleSubmit = async () => {
    if (validateForm()) {
        navigateTo('/Report_Services/');
        showModal.value = false; // Close the modal
        await saveData(); // Submit the form data
    }

    // Optionally, navigate to another page or show a success message
};

</script>

<style scoped>
.date-wrapper {
    display: flex;
    align-items: center;
}

.date-container {
    margin-right: 7px;
}

.date-container label {
    display: block;
    margin-bottom: 18px;
}

.date-container input {
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.date-container button {
    margin-top: 10px;
    padding: 5px 10px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
}
</style>
