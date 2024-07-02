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
                    <router-link to="/home/">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Home</button>
                    </router-link>

                    <div class="relative">
                        <!-- Dropdown Menu Button -->
                        <button @click="toggleDropdown" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
                            Notification
                        </button>
                        <div v-if="dropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
                            <router-link to="/Alert_Request_User/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Request</a>
                            </router-link>
                            <!-- <router-link to="/Notifications/">
                                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Services</a>
                            </router-link> -->
                        </div>
                    </div>

                    <a href="https://sites.google.com/view/takahata" target="_blank">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Shuttle booking</button>
                    </a>
                    <router-link to="/">
                        <button class="rounded-full bg-green-500 text-white px-4 py-2">Logout</button>
                    </router-link>
                </div>
            </div>
        </header>

        <!-- Services Header -->
        <div class="text-center p-4 bg-sky-300">
            <h2 class="text-2xl font-semibold">ระบบแจ้งเตือนServices</h2>
        </div>

        <!-- Search Bar -->
        <div class="search-pagination-container p-4 flex items-center justify-between">
            <!-- Search Bar -->
            <div class="search-bar flex items-center">
                <input type="text" v-model="searchQuery" placeholder="Search..."
                    class="search-input px-4 py-2 border rounded" />
                <button @click="applySearch" class="search-button px-2 py-2 rounded ml-2">Search</button>
            </div>

            <!-- Pagination -->
            <div class="pagination-container flex items-center">
                <span class="pagination-details mr-2">
                    Page {{ currentPage }} of {{ totalPages }}
                </span>
                <button @click="prevPage" class="prevPage-button px-2 py-2 rounded"
                    :disabled="currentPage === 1">Previous</button>
                <button @click="nextPage" class="nextPage-button px-2 py-2 rounded ml-2"
                    :disabled="currentPage === totalPages">Next</button>
            </div>
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
                        <th class="service-type-column">Service Type</th>
                        <th class="catalog-type-column">Catalog Type</th>
                        <th class="detail-column">Detail</th>
                        <th class="status-column">Status</th>
                        <th class="level-column">Level</th>
                        <th class="detail-admin-column">Detail Admin</th>
                        <th class="export-column">Export</th>
                    </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                    <!-- filteredItems -->
                    <tr v-for="(item, index) in displayedItems " :key="index">
                        <td class="job-no-column">{{ item.job_no }}</td>
                        <td class="en-column">{{ item.en }}</td>
                        <td class="requester-column" style="text-align: left;  padding: 8px;">{{ item.requester }}</td>
                        <td class="section-column">{{ item.section }}</td>
                        <td class="branch-column">{{ item.branch }}</td>
                        <td class="tel-column">{{ item.tel }}</td>
                        <td class="create-date-column">{{ item.create_date }}</td>
                        <td class="service-type-column">{{ item.service_type }}</td>
                        <td class="catalog-type-column">{{ item.catalog_type }}</td>
                        <!-- ปรับข้อความให้อยู่ตรงกลาง -->
                        <td class="detail-column" style="vertical-align: middle;">
                            <div
                                style="display: flex; justify-content: space-between; align-items: center; text-align: center;">
                                {{ item.detail.length > 10 ? item.detail.substring(0, 10) + '...' : item.detail }}
                                <button v-if="item.detail.length > 10" @click="showFullDetail(item)" class="more-button"
                                    style="background-color: green; color: white; margin-left: 10px;">
                                    More
                                </button>
                            </div>
                        </td>

                        <td class="status-column">
                            <div class="status-cell">
                                <span v-if="item.editing !== index" :style="{ color: getStatusColor(item.status) }">
                                    {{ item.status }}
                                </span>
                                <div v-else class="custom-dropdown">
                                    <select v-model="item.updatedStatus" class="dropdown-menu">
                                        <option value="Open" :style="{ color: 'green' }">Open</option>
                                        <option value="Wait" :style="{ color: 'black' }">Wait</option>
                                        <option value="Close" :style="{ color: 'red' }">Close</option>
                                        <option value="In Process" :style="{ color: 'orange' }">In Process</option>
                                    </select>
                                    <div class="dropdown-arrow"></div>
                                </div>

                            </div>
                        </td>
                        <!-- ปรับขนาดและกำหนดอักษร -->
                        <td class="level-column">{{ item.level }}</td>
                        <td class="detail-admin-column" style="vertical-align: middle;">
                            <div
                                style="display: flex; justify-content: space-between; align-items: center; text-align: center;">
                                {{ item.detail_admin.length > 10 ? item.detail_admin.substring(0, 10) + '...' :
                                    item.detail_admin }}
                                <div>
                                    <button v-if="item.detail_admin.length > 10" @click="showFullAdminDetail(item)"
                                        class="more-button"
                                        style="background-color: green; color: white; margin-left: 10px;">
                                        More
                                    </button>
                                </div>
                            </div>
                        </td>

                        <td class="export-column">
                            {{ item.Export }}
                            <div style="text-align: center;">
                                <button @click="downloadPDF(item)" class="pdf-button"
                                    style="text-decoration: underline; font-size: 14px;">
                                    PDF
                                </button>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Popup Modal for Detail -->
        <div v-if="showFullDetailModal" class="modal-overlay">
            <div class="modal-content">
                <button @click="showFullDetailModal = false" class="close-button">&times;</button>
                <p>{{ selectedService.detail }}</p>
            </div>
        </div>

        <!-- Popup Modal for Admin Detail -->
        <div v-if="showFullAdminDetailModal" class="modal-overlay">
            <div class="modal-content">
                <button @click="showFullAdminDetailModal = false" class="close-button">&times;</button>
                <p>{{ selectedService.detail_admin }}</p>
            </div>
        </div>

        <!-- Popup Modal for Editing Admin Note -->
        <div v-if="showModal" class="modal-overlay">
            <div class="modal-content">
                <textarea v-model="selectedService.detail_admin" rows="4" cols="50"></textarea>
                <div class="modal-actions">
                    <button @click="handleAdminNote" class="save-button">Save</button>
                    <button @click="showModal = false" class="cancel-button">Cancel</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const meme = ref([]);
const State = ref();
const showModal = ref(false);
const showFullDetailModal = ref(false);
const showFullAdminDetailModal = ref(false);
const selectedService = ref({});
const searchQuery = ref('');
const filteredItems = ref([]);
const dropdownOpen = ref(false);


import { jsPDF } from "jspdf";
import { font } from "./THSarabun-normal";

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

function toggleDropdown() {
    dropdownOpen.value = !dropdownOpen.value;
}

const downloadPDF = (index) => {
    const doc = new jsPDF();

    // Add custom font
    doc.addFileToVFS("MyFont.ttf", font);
    doc.addFont("MyFont.ttf", "MyFont", "normal");
    doc.setFont("MyFont");

    // Text strings
    const textTopRight = "INTERNTAL USE";
    const textTopCenter = "TAKAHATAPRECISION(THAILAND)LTD";
    const textTop = "ใบแจ้งระบบแจ้งซ่อม";
    const textBottomLeft = "FM-EDP-17 REV :04";
    const employeeNameTitle = "รายชื่อพนังงานเจ้าหน้าที่:";
    const itManagerNameTitle = "รายชื่อหัวหน้างานแผนกไอที:";


    // Page dimensions
    const pageWidth = doc.internal.pageSize.getWidth();
    const pageHeight = doc.internal.pageSize.getHeight();

    // Positions
    const margin = 10;
    const topMargin = 10;
    const spacing = 5;


    // Position the text "INTERNTAL USE" at the top-right corner
    doc.setFontSize(25); // Set font size for the top-right text
    const textTopRightWidth = doc.getTextWidth(textTopRight);
    doc.text(textTopRight, pageWidth - textTopRightWidth - margin, topMargin);

    // Position "TAKAHATAPRECISION(THAILAND)LTD." at the center below "INTERNTAL USE"
    doc.setFontSize(25); // Set font size for the top-center text
    const textTopCenterWidth = doc.getTextWidth(textTopCenter);
    doc.text(textTopCenter, (pageWidth - textTopCenterWidth) / 2, topMargin + spacing + 5);

    doc.setFontSize(25); // Set font size for the top text
    const textTopWidth = doc.getTextWidth(textTop);
    doc.text(textTop, (pageWidth - textTopWidth) / 2, topMargin + spacing + 15);

    // Add lines
    doc.setLineWidth(0.5);

    // Horizontal line below top texts
    doc.line(margin, topMargin + spacing + 20, pageWidth - margin, topMargin + spacing + 20);

    // Position the text "Hello world! คุญัญญา" somewhere on the page
    doc.setFontSize(20); // Set font size for the middle text

    // Job details section
    doc.text(`ผู้แจ้ง`, margin + (doc.internal.pageSize.width - doc.getStringUnitWidth(`ผู้แจ้ง`) * doc.internal.getFontSize() / 0.7) / 2, topMargin + spacing + 30);
    doc.text(`EN: ${index.en}`, margin, topMargin + spacing + 40);
    doc.text(`Requester: ${index.requester}`, margin, topMargin + spacing + 50);
    doc.text(`Section: ${index.section}`, margin, topMargin + spacing + 60);
    doc.text(`Branch : ${index.branch}`, margin, topMargin + spacing + 70);
    doc.text(`Tel: ${index.tel}`, margin, topMargin + spacing + 80);

    doc.line(margin, topMargin + spacing + 83, pageWidth - margin, topMargin + spacing + 83);

    doc.text(`รายละเอียดการแจ้ง`, margin + (doc.internal.pageSize.width - doc.getStringUnitWidth(`รายละเอียดการแจ้ง`) * doc.internal.getFontSize() / 1.7) / 2, topMargin + spacing + 90);
    doc.text(`Job No: ${index.job_no}`, margin, topMargin + spacing + 100);
    doc.text(`Create Date: ${index.create_date}`, margin, topMargin + spacing + 110);
    doc.text(`Service Type : ${index.service_type}`, margin, topMargin + spacing + 120);
    doc.text(`Catalog Type: ${index.catalog_type}`, margin, topMargin + spacing + 130);
    const detailText = `Detail: ${index.detail}`;
    const splitDetailtText = doc.splitTextToSize(detailText, pageWidth - margin * 2)
    doc.text(splitDetailtText, margin, topMargin + spacing + 140);

    doc.line(margin, topMargin + spacing + 160, pageWidth - margin, topMargin + spacing + 160);

    doc.text(`รายละเอียดไอที`, margin + (doc.internal.pageSize.width - doc.getStringUnitWidth(`รายละเอียดไอที`) * doc.internal.getFontSize() / 1.7) / 2, topMargin + spacing + 170);
    doc.text(`Technician: ${index.technician}`, margin, topMargin + spacing + 180);

    doc.text(`Closed Date: ${index.closed_date}`, margin, topMargin + spacing + 190);
    const resultsText = `Results: ${index.detail_admin}`;
    const splitResultsText = doc.splitTextToSize(resultsText, pageWidth - margin * 2);
    doc.text(splitResultsText, margin, topMargin + spacing + 200);


    // Add another horizontal line
    // doc.line(margin, topMargin + spacing + 155, pageWidth - margin, topMargin + spacing + 155);

    // Position the text "รายชื่อพนังงานเจ้าหน้าที่:" and "รายชื่อหัวหน้างานแผนกไอที:" just above "FM-EDP-17 REV :04"
    const bottomTextY = pageHeight - margin;
    doc.setFontSize(20); // Set font size for the bottom-left texts
    doc.text(`รายชื่อพนักงานเจ้าหน้าที่: ${index.technician}`, margin, bottomTextY - 25);
    doc.text(`รายชื่อหัวหน้างานแผนกไอที: Sanong Bunta `, margin, bottomTextY - 15);

    // Position the text "FM-EDP-17 REV :04" at the bottom-left corner
    doc.setFontSize(25); // Set font size for the bottom-left text
    doc.text(textBottomLeft, margin, bottomTextY);

    // Add final horizontal line at the bottom
    doc.line(margin, bottomTextY - 35, pageWidth - margin, bottomTextY - 35);

    doc.save(`ใบแจ้งระบบแจ้งซ่อม.pdf`);
};

const currentPage = ref(1);
const perPage = ref(15);

// Calculate total pages based on filteredItems length and perPage
const totalPages = computed(() => Math.ceil(filteredItems.value.length / perPage.value));

// Compute displayedItems based on currentPage and perPage
const displayedItems = computed(() => {
  const start = (currentPage.value - 1) * perPage.value;
  return filteredItems.value.slice(start, start + perPage.value);
});

// Methods for pagination
const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
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

.status-cell {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
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
    background-color: #fefffe;
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
    width: 250px;
}

.section-column {
    width: 150px;
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
    width: 250px;
}

.status-column {
    width: 120px;
}

.level-column {
    width: 100px;
}

.detail-admin-column {
    width: 250px;
}

.export-column {
    width: 100px;
}

/* Popup Modal Styles */
.modal-overlay {
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

.modal-content {
    background: white;
    padding: 20px;
    border-radius: 4px;
    text-align: center;
    position: relative;
}

.modal-actions {
    margin-top: 10px;
    display: flex;
    gap: 10px;
    justify-content: center;
}

.save-button,
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

.pagination-container {
    display: flex;
    align-items: center;
}


.pagination-details {
    margin-right: 10px;
    /* Space between the text and the buttons */
}

.prevPage-button,
.nextPage-button {
    padding: 5px 10px;
    margin-left: 5px;
    background-color: #aa3c4e;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.prevPage-button[disabled],
.nextPage-button[disabled] {
    background-color: #ccc;
    cursor: not-allowed;
}

.search-pagination-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}
</style>