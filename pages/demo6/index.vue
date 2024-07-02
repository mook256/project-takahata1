<template>
    <div>
        <title>TAKAHATA</title>

        <!-- Box content with text -->
        <header class="box-content h-8 w-50 p-4 bg-Slate-50">
            <div class="flex items-center justify-between">
                <a href="#" class="-m-1.5 p-1.5">
                    <img class="h-8 w-auto" src="/image/ex.png" alt="Logo" />
                </a>
                <div class="flex items-center space-x-2">
                    <router-link to="/home_admin/">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Home</button>
                    </router-link>
                    <a href="https://sites.google.com/view/takahata" target="_blank">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Shuttle booking</button>
                    </a>
                    <router-link to="/Notifications/">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Notification</button>
                    </router-link>
                    <router-link to="/">
                        <button class="rounded-full bg-green-500 text-white px-4 py-2">Logout</button>
                    </router-link>
                </div>
            </div>
        </header>

        <!-- Services Header -->
        <div class="text-center p-4 bg-sky-300">
            <h2 class="text-2xl font-semibold">Report-Services</h2>
        </div>



        <!-- Search Bar -->
        <div class="search-bar p-4">
            <input type="text" v-model="searchQuery" placeholder="Search..."
                class="search-input px-4 py-2 border rounded" />
            <button @click="applySearch" class="search-button px-2 py-2 bg-blue-500 text-white rounded">
                Search
            </button>
        </div>

        <!-- Data Display Table -->
        <div class="table-container">
            <table class="table">
                <thead class="bg-gray-50">
                    <tr>
                        <th class="job-no-column">Job No</th>
                        <th class="en-column">EN</th>
                        <th class="requester-column">Requester</th>
                        <th class="section-column">Section</th>
                        <th class="branch-column">Branch</th>
                        <th class="tel-column">Tel</th>
                        <th class="create-date-column">Create Date</th>
                        <th class="detail-column">Detail</th>
                        <th class="level-column">Level</th>
                        <th class="status-column">Status</th>
                        <th class="technician-column ">Technician</th>
                        <th class="detail-admin-column text-rose-500">*Results*</th>
                        <th class="due_by-column ">Closed Date</th>
                        <th class="export-column">Export</th>
                    </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                    <tr v-for="(item, index) in filteredItems" :key="index">
                        <td class="job-no-column">{{ item.job_no }}</td>
                        <td class="en-column">{{ item.en }}</td>
                        <td class="requester-column" style="text-align: left;  padding: 8px;">{{ item.requester }}</td>
                        <td class="section-column">{{ item.section }}</td>
                        <td class="branch-column">{{ item.branch }}</td>
                        <td class="tel-column">{{ item.tel }}</td>
                        <td class="create-date-column">{{ item.create_date }}</td>
                        <!-- <td class="service-type-column">{{ item.service_type }}</td>
                        <td class="catalog-type-column">{{ item.catalog_type }}</td> -->
                        <td class="detail-column">
                            <button class="underline decoration-red-600 " @click="showFullDetail(item)">Show
                                Detail</button>
                        </td>
                        <td class="level-column">{{ item.level }}</td>
                        <td class="status-column">
                            <div class="status-cell">
                                <span v-if="item.editing !== index" :style="{ color: getStatusColor(item.status) }"
                                    class="status-text">
                                    {{ item.status }}
                                </span>


                            </div>
                        </td>
                        <!--  -->
                        <td class="technician-column" style="text-align: left;  padding: 8px;">{{ item.technician }}</td>

                        <td class="detail-admin-column" style="vertical-align: middle;">
                            <div
                                style="display: flex; justify-content: center; align-items: center; text-align: center;">
                                <span>
                                    {{ item.detail_admin.length > 10 ? item.detail_admin.substring(0, 10) + '...' :
                                        item.detail_admin }}
                                </span>
                                <div class="admin-detail-actions">
                                    <button v-if="item.detail_admin.length > 10" @click="showFullAdminDetail(item)"
                                        class="more-button" style="background-color: green; color: white; ">
                                        More
                                    </button>

                                </div>
                            </div>
                        </td>
                        <!--  -->
                        <td class="due_by-column">{{ item.closed_date }}</td>
                        <td class="export-column">
                            {{ item.Export }}
                            <div style="text-align: center;">
                            

                           
                            </div>
                           
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Popup Modal for Detail Service -->
        <div v-if="showFullDetailModal" class="modal-overlay_S">
            <div class="modal-content scrollable-content">
                <button @click="showFullDetailModal = false" class="close-button">&times;</button>
                <h2 class="title">User Services</h2>
                <h3 class="sub-topic">Service Type</h3>
                <p style="text-align: left;">{{ selectedService.service_type }}</p>

                <h3 class="sub-topic">Catalog Type</h3>
                <p style="text-align: left;">{{ selectedService.catalog_type }}</p>

                <h3 class="sub-topic">Detail</h3>
                <p style="text-align: left;">{{ selectedService.detail }}</p>
            </div>
        </div>

        <!-- Popup Modal for Admin Detail -->
        <div v-if="showFullAdminDetailModal" class="modal-overlay_S">
            <div class="modal-content">
                <button @click="showFullAdminDetailModal = false" class="close-button">&times;</button>
                <p>{{ selectedService.detail_admin }}</p>
            </div>
        </div>

        <!-- Popup Modal for Editing Admin Note -->
        <div v-if="showModal" class="modal-overlay_S">
            <div class="modal-content">
                <textarea v-model="selectedService.detail_admin" rows="4" cols="50"></textarea>
                <div class="modal-actions">
                    <button @click="handleAdminNote" class="save-button">Submit</button>
                    <button @click="showModal = false" class="cancel-button">Cancel</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { jwtDecode } from "jwt-decode";
// import {demofilepde1} from "../demofilepde1/index.vue"
const token = jwtDecode(useCookie("token").value); // nuxt cookie & jwtDecode nuxt

const meme = ref([]);
const State = ref();
const showModal = ref(false);
const showFullDetailModal = ref(false);
const showFullAdminDetailModal = ref(false);
const selectedService = ref({});
const searchQuery = ref('');
const filteredItems = ref([]);

const test_data = async () => {
    try {
        const response = await fetch('http://172.29.10.238:8080/api/IT_Repair_data_table', {
            method: 'POST',
            credentials: 'include',
        });
        const data = await response.json();
        meme.value = data.results;
        filteredItems.value = meme.value;
        console.log(meme.value)
    } catch (error) {
        console.error('Failed to fetch data:', error);
    }
};

test_data();

const status_update = async (index) => {
    try {
        const response = await fetch('http://172.29.10.238:8080/api/IT_Repair_status_choice', {
            method: 'POST',
            body: JSON.stringify({
                data: meme.value[index].status,
                job_no: meme.value[index].job_no,

            }),
            headers: {
                'Content-Type': 'application/json',
            },
            credentials: 'include',
        });
        const result = await response.json();
        State.value = result.results;
        console.log(State.value);
    } catch (error) {
        console.error('Failed to fetch test_api data:', error);
    }
};

const getStatusColor = (status) => {
    switch (status) {
        case 'Open':
            return 'green';
        case 'Wait':
            return 'black';
        case 'Close':
            return 'red';
        case 'In Process':
            return 'orange';
        default:
            return 'green';
    }
};

const editStatus = (index) => {
    meme.value[index].editing = index;
    meme.value[index].updatedStatus = meme.value[index].status;
};

const cancelEdit = (index) => {
    meme.value[index].editing = -1;
};

const applyEdit = (index) => {
    meme.value[index].status = meme.value[index].updatedStatus;
    meme.value[index].editing = -1;
    status_update(index);
};

const saveAdminNote = async () => {
    try {
        await fetch('http://172.29.10.238:8080/api/save_admin_note', {
            method: 'POST',
            body: JSON.stringify({
                admin_note: selectedService.value.detail_admin,
                job_no: selectedService.value.job_no,
            }),
            headers: {
                'Content-Type': 'application/json',
            },
            credentials: 'include',
        });
    } catch (error) {
        console.error('Failed to save admin note:', error);
    }
};

const handleAdminNote = async () => {
    showModal.value = false;

    // Update the relevant item in the meme array
    const itemIndex = meme.value.findIndex(item => item.job_no === selectedService.value.job_no);
    if (itemIndex !== -1) {
        meme.value[itemIndex].detail_admin = selectedService.value.detail_admin;
    }

    await saveAdminNote();
};

const applySearch = () => {
    filteredItems.value = meme.value.filter(item => {
        return Object.values(item).some(value =>
            String(value).toLowerCase().includes(searchQuery.value.toLowerCase())
        );
    });
};

const openPopup = (item) => {
    selectedService.value = { ...item };
    showModal.value = true;
};

const showFullDetail = (item) => {
    selectedService.value = { ...item };
    showFullDetailModal.value = true;
};

const showFullAdminDetail = (item) => {
    selectedService.value = { ...item };
    showFullAdminDetailModal.value = true;
};
</script>

<style>
table {
    width: 100%;
    table-layout: fixed;
    border-collapse: collapse;
}

th,
td {
    text-align: center;
    border: 1px solid #ccc;
}

.table-container {
    width: 100%;
    overflow-x: auto;
}

/*  */
.sub-topic {
    font-weight: bold;
    text-align: left;
    /* Align the sub-topics to the left */
    margin-bottom: 10px;
    /* Add some space below each sub-topic */
}

.title {
    font-size: 24px;
    text-decoration: underline;
}

.search-bar {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    gap: 10px;
    margin: 10px 0;
    background-color: #ea9de0;
}

.status-cell {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    position: relative;
}

.status-text {
    flex: 1;
    text-align: center;
}

.edit-button {
    background-color: #007bff;
    color: #fff;
    padding: 5px 10px;
    border: none;
    cursor: pointer;
}

.custom-dropdown {
    display: flex;
    align-items: center;
    gap: 10px;
}

.action-buttons {
    display: flex;
    gap: 5px;
    justify-content: center;
    align-items: center;
}

.search-bar {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    gap: 10px;
    margin: 10px 0;
}

.search-input {
    width: 200px;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.search-button {
    padding: 5px 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

/* Column width classes */
.job-no-column {
    width: 100px;
}

.en-column {
    width: 100px;
}

.requester-column {
    width: 200px;
}

.section-column {
    width: 100px;
}

.branch-column {
    width: 100px;
}

.tel-column {
    width: 100px;
}

.create-date-column {
    width: 150px;
}

.service-type-column {
    width: 150px;
}

.catalog-type-column {
    width: 150px;
}

.detail-column {
    width: 100px;
}

.status-column {
    width: 100px;
}

.level-column {
    width: 100px;
}

.detail-admin-column {
    width: 250px;
}

.technician-column {
    width: 250px;
}

.due_by-column {
    width: 150px;
}

.export-column {
    width: 100px;
}



/* Popup Modal Styles */
.modal-overlay_S {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.scrollable-content {
    max-height: 80vh;
    overflow-y: auto;
    padding-right: 15px;
}

.modal-content {
    position: relative;
    background: white;
    padding: 20px;
    border-radius: 8px;
    width: 420px;
}

.modal-actions {
    margin-top: 10px;
    display: flex;
    gap: 10px;
    justify-content: center;
}

.save-button,

.apply-button {
    padding: 1px 1px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.cancel-button {
    padding: 1px 1px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.save-button {
    background-color: green;
    color: white;
}

.cancel-button {
    background-color: red;
    color: white;
}

.more-button {
    background-color: green;
    color: white;
    padding: 5px 1px;
    border: none;
    cursor: pointer;
    margin-right: 10px;
}

.close-button {
    position: absolute;
    top: 10px;
    right: 10px;
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
}

.admin-detail-actions {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    flex: 1;
}
</style>