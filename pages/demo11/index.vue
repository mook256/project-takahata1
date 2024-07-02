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

                    <router-link to="/Notifications/">
                        <button class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Notification</button>
                    </router-link>
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
            <h2 class="text-2xl font-semibold">Request</h2>
        </div>

        <!-- Search Bar -->
        <div class="search-bar p-4">
            <input type="text" v-model="searchQuery" placeholder="Search..."
                class="search-input px-4 py-2 border rounded" />
            <button @click="applySearch" class="search-button px-2 py-2 bg-blue-500 text-white rounded">Search</button>
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
                        <th class="request-approval-column">Request Status</th>
                        <th class="detail-manager-column text-rose-500">* Reason *</th>
                        <th class="status-column">Status</th>

                        <th class="detail-admin-column text-rose-500">* Results * </th>
                        <th class="export-column">Export</th>
                    </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                    <tr v-for="(item, index) in filteredItems" :key="index">
                        <td class="job-no-column">{{ item.job_no }}</td>
                        <td class="en-column">{{ item.en }}</td>
                        <td class="requester-column">{{ item.requester }}</td>
                        <td class="section-column">{{ item.section }}</td>
                        <td class="branch-column">{{ item.branch }}</td>
                        <td class="tel-column">{{ item.tel }}</td>
                        <td class="create-date-column">{{ item.create_date }}</td>
                        <td class="detail-column">
                            <button class="underline decoration-red-600 " @click="showFullDetail(item)">Show
                                Detail</button>
                        </td>
                        <td class="level-column">{{ item.level }}</td>
                        <td class="request-approval-column">
                            <div class="status-cell">
                                <span v-if="item.editingApproval !== index"
                                    :style="{ color: getManagerColor(item.request_approval) }" class="status-text">
                                    {{ item.request_approval }}
                                </span>



                            </div>
                        </td>
                        <td class="detail-manager-column">
                            <div
                                style="display: flex; justify-content: space-between; align-items: center; text-align: right;">
                                <span>
                                    {{ item.detail_manager.length > 10 ? item.detail_manager.substring(0, 10) + '...' :
                                        item.detail_manager }}
                                </span>
                                <div style="display: flex; flex-direction: row; align-items: center;">
                                    <button v-if="item.detail_manager.length > 10" @click="showFullManagerDetail(item)"
                                        class="more-button" style="background-color: green; color: white;">
                                        More
                                    </button>

                                </div>
                            </div>
                        </td>
                        <td class="status-column">
                            <div class="status-cell">
                                <span v-if="item.editingStatus !== index"
                                    :style="{ color: getStatusColor(item.status) }" class="status-text">
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
                                <button v-if="item.editingStatus !== index" @click="editStatus(index)"
                                    class="edit-button"
                                    style="background-color: blue; color: white; width: 50px; margin-right: 10px;">
                                    Edit
                                </button>
                                <div v-if="item.editingStatus === index" class="action-buttons">
                                    <button @click="applyStatusEdit(index)" class="apply-button"
                                        style="background-color: green; color: white; width: 65px;">
                                        Apply
                                    </button>
                                    <button @click="cancelStatusEdit(index)" class="cancel-button"
                                        style="background-color: red; color: white; width: 65px;">
                                        Cancel
                                    </button>
                                </div>
                            </div>
                        </td>

                        <td class="detail-admin-column" style="vertical-align: middle;">
                            <div
                                style="display: flex; justify-content: space-between; align-items: center; text-align: center;">
                                <span>
                                    {{ item.detail_admin.length > 10 ? item.detail_admin.substring(0, 10) + '...' :
                                        item.detail_admin }}
                                </span>
                                <div style="display: flex; flex-direction: row; align-items: center;">
                                    <button v-if="item.detail_admin.length > 10" @click="showFullAdminDetail(item)"
                                        class="more-button" style="background-color: green; color: white;">
                                        More
                                    </button>
                                    <button @click="openPopup(item)" class="edit-button"
                                        style="background-color: blue; color: white; margin-right: 10px;">
                                        Edit
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

        <!-- Popup Modal for Details -->
        <div v-if="showFullDetailModal" class="modal-overlay">
            <div class="modal-content scrollable-content">
                <button @click="showFullDetailModal = false" class="close-button">&times;</button>
                <h2 class="title">User Request</h2>
                <h3 class="sub-topic">User Account</h3>
                <div>
                    <ul>
                        <li v-for="(item, index) in selectedService.user_account.split(',')" :key="index">
                            <template v-if="item">· {{ item }}</template>
                            <template v-else>·</template>
                        </li>
                    </ul>
                </div>
                <p style="text-align: left;">Name: {{ selectedService.detail_user }}</p>

                <h3 class="sub-topic">Hardware</h3>
                <ul>
                    <li v-for="(item, index) in selectedService.hardware.split(',')" :key="index">
                        <template v-if="item">· {{ item }}</template>
                        <template v-else>·</template>
                    </li>
                </ul>

                <h3 class="sub-topic">Software License</h3>
                <ul>
                    <li v-for="(item, index) in selectedService.software_license.split(',')" :key="index">
                        <template v-if="item">· {{ item }}</template>
                        <template v-else>·</template>
                    </li>
                </ul>

                <h3 class="sub-topic">ERP&MRP</h3>
                <ul>
                    <li v-for="(item, index) in selectedService.erp_mrp.split(',')" :key="index">
                        <template v-if="item">· {{ item }}</template>
                        <template v-else>·</template>
                    </li>
                </ul>

                <h3 class="sub-topic">File Server</h3>
                <ul>
                    <li v-for="(item, index) in selectedService.file_server.split(',')" :key="index">
                        <template v-if="item">· {{ item }}</template>
                        <template v-else>·</template>
                    </li>
                </ul>

                <p style="text-align: left;">File: {{ selectedService.detail_file }}</p>

                <h3 class="sub-topic">Installation</h3>
                <ul>
                    <li v-for="(item, index) in selectedService.installation.split(',')" :key="index">
                        <template v-if="item">· {{ item }}</template>
                        <template v-else>·</template>
                    </li>
                </ul>

                <h3 class="sub-topic">Detail</h3>
                <p style="text-align: left;">{{ selectedService.detail }}</p>
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
                    <button @click="handleAdminNote" class="save-button">Submit</button>
                    <button @click="showModal = false" class="cancel-button">Cancel</button>
                </div>
            </div>
        </div>

        <!-- Popup Modal for Manager Detail -->
        <div v-if="showFullManagerDetailModal" class="modal-overlay">
            <div class="modal-content">
                <button @click="showFullManagerDetailModal = false" class="close-button">&times;</button>
                <p>{{ selectedService.detail_manager }}</p>
            </div>
        </div>

        <!-- Popup Modal for Editing Manager Note -->
        <div v-if="showManagerModal" class="modal-overlay">
            <div class="modal-content">
                <textarea v-model="selectedService.detail_manager" rows="4" cols="50"></textarea>
                <div class="modal-actions">
                    <button @click="handleManagerNote" class="save-button">Submit</button>
                    <button @click="showManagerModal = false" class="cancel-button">Cancel</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { jwtDecode } from "jwt-decode";
const token = jwtDecode(useCookie("token").value); // nuxt cookie & jwtDecode nuxt

import { jsPDF } from "jspdf";
import { font } from "./THSarabun-normal";

const meme = ref([]);
const showModal = ref(false);
const showManagerModal = ref(false);
const showFullDetailModal = ref(false);
const showFullAdminDetailModal = ref(false);
const showFullManagerDetailModal = ref(false);
const selectedService = ref({});
const searchQuery = ref('');
const filteredItems = ref([]);
const tableData = ref([]);
const currentDataIndex = ref(null);

console.log(token.name)

const testClick = async () => {
    try {
        const response = await fetch('http://172.29.10.238:8080/api/IT_Request_TableController', {
            method: 'POST',
            credentials: 'include'
        });
        const data2 = await response.json();
        tableData.value = data2.results;
    } catch (error) {
        console.error('Failed to fetch data:', error);
    }
};

const Show_On_Table = async () => {
    try {
        const response = await fetch('http://172.29.10.238:8080/api/IT_Request_TableAdminController', {
            method: 'POST',
            credentials: 'include',
        });
        const data = await response.json();
        meme.value = data.results;
        filteredItems.value = meme.value;
    } catch (error) {
        console.error('Failed to fetch data:', error);
    }
};

Show_On_Table();

const Status_update = async (index) => {
    try {
        await fetch('http://172.29.10.238:8080/api/IT_Request_StatusController', {
            method: 'POST',
            body: JSON.stringify({
                data: meme.value[index].status,
                job_no: meme.value[index].job_no,
                admin_name: token.name
            }),
            headers: {
                'Content-Type': 'application/json',
            },
            credentials: 'include',
        });
    } catch (error) {
        console.error('Failed to fetch data:', error);
    }
};

const Approval_update = async (index) => {
    try {
        await fetch('http://172.29.10.238:8080/api/IT_Request_ApprovalController', {
            method: 'POST',
            body: JSON.stringify({
                data1: meme.value[index].request_approval,
                job_no: meme.value[index].job_no,
            }),
            headers: {
                'Content-Type': 'application/json',
            },
            credentials: 'include',
        });
    } catch (error) {
        console.error('Failed to fetch data:', error);
    }
};

const getManagerColor = (request) => {
    switch (request) {
        case 'Wait':
            return 'green';
        case 'Approve':
            return 'blue';
        case 'Cancel':
            return 'red';
        default:
            return 'green';
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

const editApproval = (index) => {
    meme.value[index].editingApproval = index;
    meme.value[index].updatedRequest = meme.value[index].request_approval;
};

const cancelApprovalEdit = (index) => {
    meme.value[index].editingApproval = -1;
};

const applyApprovalEdit = (index) => {
    meme.value[index].request_approval = meme.value[index].updatedRequest;
    meme.value[index].editingApproval = -1;
    Approval_update(index);
};

const editStatus = (index) => {
    meme.value[index].editingStatus = index;
    meme.value[index].updatedStatus = meme.value[index].status;
};

const cancelStatusEdit = (index) => {
    meme.value[index].editingStatus = -1;
};

const applyStatusEdit = (index) => {
    meme.value[index].status = meme.value[index].updatedStatus;
    meme.value[index].editingStatus = -1;
    Status_update(index);
};

const saveAdminNote = async () => {
    try {
        await fetch('http://172.29.10.238:8080/api/IT_Request_AdminDetailController', {
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

const saveManagerNote = async () => {
    try {
        await fetch('http://172.29.10.238:8080/api/IT_Request_ManagerDetailController', {
            method: 'POST',
            body: JSON.stringify({
                manager_note: selectedService.value.detail_manager,
                job_no: selectedService.value.job_no,
            }),
            headers: {
                'Content-Type': 'application/json',
            },
            credentials: 'include',
        });
    } catch (error) {
        console.error('Failed to save manager note:', error);
    }
};

const handleAdminNote = async () => {
    showModal.value = false;
    const itemIndex = meme.value.findIndex(item => item.job_no === selectedService.value.job_no);
    if (itemIndex !== -1) {
        meme.value[itemIndex].detail_admin = selectedService.value.detail_admin;
    }
    await saveAdminNote();
};

const handleManagerNote = async () => {
    showManagerModal.value = false;
    const itemIndex = meme.value.findIndex(item => item.job_no === selectedService.value.job_no);
    if (itemIndex !== -1) {
        meme.value[itemIndex].detail_manager = selectedService.value.detail_manager;
    }
    await saveManagerNote();
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

const openManagerPopup = (item) => {
    selectedService.value = { ...item };
    showManagerModal.value = true;
};

const showFullDetail = (item) => {
    selectedService.value = item;
    currentDataIndex.value = tableData.value.findIndex(ite => ite === item);
    showFullDetailModal.value = true;
};

const showFullAdminDetail = (item) => {
    selectedService.value = { ...item };
    showFullAdminDetailModal.value = true;
};

const showFullManagerDetail = (item) => {
    selectedService.value = { ...item };
    showFullManagerDetailModal.value = true;
};

const downloadPDF = (index) => {
    const doc = new jsPDF();

    let user_account_arr, hardware_arr, software_license_arr, erp_mrp_arr, file_server_arr, installation_arr
    let temp, n = 0




    // Add custom font
    doc.addFileToVFS("MyFont.ttf", font);
    doc.addFont("MyFont.ttf", "MyFont", "normal");
    doc.setFont("MyFont");

    // Text strings
    const textTopRight = "INTERNAL USE";
    const textTopCenter = "TAKAHATAPRECISION(THAILAND)LTD";
    const textTop = "ใบคำร้องขอใช้ระบบ";
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
    const col2Start = pageWidth / 2;
    // Add lines
    doc.setLineWidth(0.5);

    // Horizontal line below top texts
    doc.line(margin, topMargin + spacing + 20, pageWidth - margin, topMargin + spacing + 20);

    // Position the text "Hello world! คุญัญญา" somewhere on the page

    doc.setFontSize(20); // ตั้งค่าขนาดตัวอักษร
    doc.text(`ผู้แจ้ง`, margin + (doc.internal.pageSize.width - doc.getStringUnitWidth(`ผู้แจ้ง`) * doc.internal.getFontSize() / 0.7) / 2, topMargin + spacing + 30);
    doc.text(`EN: ${index.en}`, margin, topMargin + spacing + 40);
    doc.text(`Requester: ${index.requester}`, margin, topMargin + spacing + 50);
    doc.text(`Section: ${index.section}`, margin, topMargin + spacing + 60);
    doc.text(`Branch : ${index.branch}`, col2Start, topMargin + spacing + 40);
    doc.text(`Tel: ${index.installation}`, col2Start, topMargin + spacing + 50);

    doc.line(margin, topMargin + spacing + 70, pageWidth - margin, topMargin + spacing + 70);

    doc.text(`รายละเอียดการแจ้ง`, margin + (doc.internal.pageSize.width - doc.getStringUnitWidth(`รายละเอียดการแจ้ง`) * doc.internal.getFontSize() / 1.7) / 2, topMargin + spacing + 80);
    doc.text(`Job No: ${index.job_no}`, margin, topMargin + spacing + 90);
    doc.text(`Create Date: ${index.create_date}`, col2Start, topMargin + spacing + 90);
    doc.text(`Topic`, margin, topMargin + spacing + 100);
    //doc.text(`Topic : ${index.user_account}`, margin, topMargin + spacing + 100);


    if (index.user_account != "") {
        n++
        temp = index.user_account
        user_account_arr = ["User Account", ...temp.split(",")]
    }
    if (index.hardware != "") {
        n++
        temp = index.hardware
        hardware_arr = ["Hardware", ...temp.split(",")]
    }
    if (index.software_license != "") {
        n++
        temp = index.software_license
        software_license_arr = ["Software License", ...temp.split(",")]
    }
    if (index.erp_mrp != "") {
        n++
        temp = index.erp_mrp
        erp_mrp_arr = ["ERP&MRP", ...temp.split(",")]
    }
    if (index.file_server != "") {
        n++
        temp = index.file_server
        file_server_arr = ["File Server", ...temp.split(",")]
    }
    if (index.installation != "") {
        n++
        temp = index.installation
        installation_arr = ["Installation", ...temp.split(",")]
    }

    let row_item
    let start_item = 100
    let start_item_r = 100
    let i = 1
    let flag = [0, 0, 0, 0, 0, 0]
    row_item = n / 2
    row_item = Math.ceil(row_item)

    console.log(row_item)

    //margin, topMargin + spacing + 100
    while (i <= row_item) {
        console.log("In while")
        if (index.user_account != "") {//
            flag[0] = 1;
            doc.text(`${user_account_arr[0]}`, margin, topMargin + spacing + (start_item += 10));
            for (var j = 1; j < user_account_arr.length; j++) {
                doc.text(`- ${user_account_arr[j]}`, margin, topMargin + spacing + (start_item += 10));
            }
            doc.text(` Username: ${index.detail_user}`, margin, topMargin + spacing + (start_item += 10));
            i++
            if (i >= row_item) break;
        }
        console.log(i)
        if (index.hardware != "") {
            flag[1] = 1;
            doc.text(`${hardware_arr[0]}`, margin, topMargin + spacing + (start_item += 10));
            for (var j = 1; j < hardware_arr.length; j++) {
                doc.text(`- ${hardware_arr[j]}`, margin, topMargin + spacing + (start_item += 10));
            }
            i++
            if (i >= row_item) break;
        }
        console.log(i)
        if (index.software_license != "") {
            flag[2] = 1;
            doc.text(`${software_license_arr[0]}`, margin, topMargin + spacing + (start_item += 10));
            for (var j = 1; j < software_license_arr.length; j++) {
                doc.text(`- ${software_license_arr[j]}`, margin, topMargin + spacing + (start_item += 10));
            }
            i++
            if (i >= row_item) break;
        }
        console.log(i)
        if (index.erp_mrp != "") {
            flag[3] = 1;
            doc.text(`${erp_mrp_arr[0]}`, margin, topMargin + spacing + (start_item += 10));
            for (var j = 1; j < erp_mrp_arr.length; j++) {
                doc.text(`- ${erp_mrp_arr[j]}`, margin, topMargin + spacing + (start_item += 10));
            }
            i++
            if (i >= row_item) break;
        }
        console.log(i)
        if (index.file_server != "") {//
            flag[4] = 1;
            doc.text(`${file_server_arr[0]}`, margin, topMargin + spacing + (start_item += 10));
            for (var j = 1; j < file_server_arr.length; j++) {
                doc.text(`- ${file_server_arr[j]}`, margin, topMargin + spacing + (start_item += 10));
            }
            doc.text(`File name: ${index.detail_file}`, margin, topMargin + spacing + (start_item += 10));
            i++
            if (i >= row_item) break;
        }
        console.log(i)
        if (index.installation != "") {
            flag[5] = 1;
            doc.text(`${installation_arr[0]}`, margin, topMargin + spacing + (start_item += 10));
            for (var j = 1; j < installation_arr.length; j++) {
                doc.text(`- ${installation_arr[j]}`, margin, topMargin + spacing + (start_item += 10));
            }
            i++
            if (i >= row_item) break;
        }
        console.log(i)

    }

    if (index.hardware != "" && flag[1] != 1) {
        doc.text(`${hardware_arr[0]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        for (var j = 1; j < hardware_arr.length; j++) {
            doc.text(`- ${hardware_arr[j]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        }
        i++

    }
    if (index.software_license != "" && flag[2] != 1) {
        doc.text(`${software_license_arr[0]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        for (var j = 1; j < software_license_arr.length; j++) {
            doc.text(`- ${software_license_arr[j]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        }
        i++
    }
    if (index.erp_mrp != "" && flag[3] != 1) {
        doc.text(`${erp_mrp_arr[0]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        for (var j = 1; j < erp_mrp_arr.length; j++) {
            doc.text(`- ${erp_mrp_arr[j]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        }
        i++
    }
    if (index.file_server != "" && flag[4] != 1) {//
        doc.text(`${file_server_arr[0]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        for (var j = 1; j < file_server_arr.length; j++) {
            doc.text(`- ${file_server_arr[j]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        }
        doc.text(`File name: ${index.detail_file}`, col2Start, topMargin + spacing + (start_item_r += 10));
        i++
    }
    if (index.installation != "" && flag[5] != 1) {
        doc.text(`${installation_arr[0]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        for (var j = 1; j < installation_arr.length; j++) {
            doc.text(`- ${installation_arr[j]}`, col2Start, topMargin + spacing + (start_item_r += 10));
        }
        i++
    }
    /*   for (let i = 0; i < row_item; i++){
           
       }*/

    const detailText = `Detail: ${index.detail}`;
    const splitDetailtText = doc.splitTextToSize(detailText, pageWidth - margin * 2)
    doc.text(splitDetailtText, margin, topMargin + spacing + 140);

    doc.line(margin, topMargin + spacing + 160, pageWidth - margin, topMargin + spacing + 160);

    //   doc.text(`รายละเอียดการแจ้ง`, margin + (doc.internal.pageSize.width - doc.getStringUnitWidth(`รายละเอียดการแจ้ง`) * doc.internal.getFontSize() / 1.7) / 2, topMargin + spacing + 80);
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
    doc.text(`รายชื่อพนักงาน: ${index.requester}`, margin, bottomTextY - 32);
    doc.text(`รายชื่อหัวหน้างาน: ${index.manager_name}`, margin, bottomTextY - 25);
    doc.text(`รายชื่อพนักงานเจ้าหน้าที่: ${index.technician}`, margin, bottomTextY - 17);
    doc.text(`รายชื่อหัวหน้างานแผนกไอที: Sanong Bunta `, margin, bottomTextY - 10);

    // Position the text "FM-EDP-17 REV :04" at the bottom-left corner
    doc.setFontSize(25); // Set font size for the bottom-left text
    doc.text(textBottomLeft, margin, bottomTextY);

    // Add final horizontal line at the bottom
    doc.line(margin, bottomTextY - 40, pageWidth - margin, bottomTextY - 40);

    doc.save(`ใบคำร้องขอใช้ระบบ.pdf`);
};
</script>

<style scoped>
.table-container {
    width: 100%;
    overflow-x: auto;
}

.table {
    width: 100%;
    table-layout: fixed;
    border-collapse: collapse;
}

th,
td {
    text-align: center;
    border: 1px solid #ccc;
}

.status-cell {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    position: relative;
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
    background-color: #63c68c;
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
    width: 120px;
}

.en-column {
    width: 100px;
}

.requester-column {
    width: 250px;
}

.section-column {
    width: 90px;
}

.branch-column {
    width: 90px;
}

.tel-column {
    width: 90px;
}

.create-date-column {
    width: 170px;
}

.request-approval-column {
    width: 250px;
}

.detail-manager-column {
    width: 250px;
}

.detail-column {
    width: 250px;
}

.status-column {
    width: 250px;
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
    position: relative;
    background: white;
    padding: 20px;
    border-radius: 8px;
    width: 420px;
}

.scrollable-content {
    max-height: 80vh;
    overflow-y: auto;
    padding-right: 15px;
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

.title {
    font-size: 24px;
    text-decoration: underline;
}

.sub-topic {
    font-weight: bold;
    text-align: left;
    /* Align the sub-topics to the left */
    margin-bottom: 10px;
    /* Add some space below each sub-topic */
}

.modal-content ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
    text-align: left;
    /* Align the list items to the left */
}

.modal-content ul li {
    margin-bottom: 5px;
}

.modal-actions {
    margin-top: 10px;
    display: flex;
    gap: 10px;
    justify-content: center;
}

.save-button,
.apply-button {
    padding: 5px 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.cancel-button {
    padding: 5px 10px;
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
    padding: 5px 10px;
    border: none;
    cursor: pointer;
    margin-right: 10px;
}

.status-text {
    flex: 1;
    text-align: center;
}
</style>