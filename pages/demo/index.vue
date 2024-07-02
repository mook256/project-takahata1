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
          <h2 class="text-2xl font-semibold">Report </h2>
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
              <div class="date-wrapper"> <!--v-if="selectedIssue"-->
                <div class="date-container ">
                  <label for="date " class="block mb-2 font-bold">Start Date:</label>
                  <input type="date" v-model="selectedDateStart">
                </div>
                <div class="date-container">
                  <label for="date1" class="block mb-2 font-bold">End Date</label>
                  <input type="date" v-model="selectedDateEnd">
                  <!--<button @click="refreshPage">Refresh test</button>-->
                </div>
              </div>


              <!-- Buttons -->
              <div class="mt-6">
                <div class="flex items-center justify-center space-x-2">

                </div>
              </div>

            </form>
            <!-- Buttons -->
            <div class="flex items-center justify-center space-x-2 flex-grow">
              <button @click="Search_Show" class="rounded-full bg-green-500 text-white px-4 py-2">แสดงตาราง</button>
            </div>

            <!-- @click="showModal" -->


            <!-- Buttons -->
            <div class="mt-6">
              <div class="flex items-center justify-center space-x-2">

              </div>
            </div>

          </div>
        </div>
      </main>

    </div>
    <div>

      <div class="container box-border mx-auto max-w-prose bg-white p-0.1">
        <div class="text-center mb-8">
          <div class="flex flex-col items-center ">

          </div>
        </div>
      </div>

      <div class="container box-border mx-auto max-w-prose bg-white p-0.1">
        <div class="text-center mb-8">
          <div class="flex flex-col items-center ">
            <h2 class="text-2xl font-semibold ">Report by: {{ selectedTopic }} - Group by: {{ selectedIssue }}</h2>
          </div>
        </div>
      </div>



      <div>
        <div class="search-pagination-container p-4 flex items-center justify-between">
          <!-- Search Bar -->
          <div class="search-bar flex items-center">
            <input type="text" v-model="searchQuery" placeholder="Search..."
              class="search-input px-4 py-2 border rounded" />
            <button @click="applySearch" class="search-button px-2 py-2 rounded ml-2">Search</button>
            <button @click="downloadPDF" class="download-button px-2 py-2 rounded ml-2">Download PDF</button>
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




        <div>

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
                  <th class="request-approval-column">Status</th>
                  <th class="technician-column ">Technician</th>
                  <th class="detail-admin-column text-rose-500">* Results * </th>
                  <th class="due_by-column ">Closed Date</th>

                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr v-for="(item, index) in paginatedData" :key="index">
                  <td class="job-no-column">{{ item.job_no }}</td>
                  <td class="en-column">{{ item.en }}</td>
                  <td class="requester-column" style="text-align: left;  padding: 8px;">{{ item.requester }}</td>
                  <td class="section-column">{{ item.section }}</td>
                  <td class="branch-column">{{ item.branch }}</td>
                  <td class="tel-column">{{ item.tel }}</td>
                  <td class="create-date-column">{{ item.create_date }}</td>
                  <td class="detail-column">
                    <button v-if="item.service_type !== null && item.service_type !== ''"
                      class="underline decoration-red-600" @click="showFullDetail_S(item)">
                      Show Detail
                    </button>
                    <button v-else class="underline decoration-red-600" @click="showFullDetail(item)">
                      Show Detail
                    </button>
                  </td>

                  <td class="level-column">{{ item.level }}</td>
                  <td class="request-approval-column">
                    <div class="status-text" :style="{ color: getStatusColor(item.status) }">{{ item.status }} </div>
                  </td>


                  <td class="technician-column" style="text-align: left;  padding: 8px;">{{ item.technician }}</td>

                  <td class="detail-admin-column" style="vertical-align: middle;">
                    <div
                      style="display: flex; justify-content: space-between; align-items: center; text-align: center;">
                      <span>
                        {{ item.detail_admin.length > 10 ? item.detail_admin.substring(0, 10) + '...' :
                          item.detail_admin }}
                      </span>
                      <div style="display: flex; flex-direction: row; align-items: center;">

                      </div>
                    </div>
                  </td>
                  <td class="due_by-column">{{ item.closed_date }}</td>

                </tr>
              </tbody>
            </table>
          </div>
        </div>

      </div>
    </div>
    <!-- Popup Modal for Detail Service -->
    <div v-if="showFullDetailModal_S" class="modal-overlay">
      <div class="modal-content scrollable-content">
        <button @click="showFullDetailModal_S = false" class="close-button">&times;</button>
        <h2 class="title container">User Services</h2>
        <h3 class="sub-topic">Service Type</h3>
        <p style="text-align: left;">{{ selectedService_S.service_type }}</p>

        <h3 class="sub-topic">Catalog Type</h3>
        <p style="text-align: left;">{{ selectedService_S.catalog_type }}</p>

        <h3 class="sub-topic">Detail</h3>
        <p style="text-align: left;">{{ selectedService_S.detail }}</p>
      </div>
    </div>

    <!-- Popup Modal for Details Request -->
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
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { jwtDecode } from "jwt-decode";
import { useRouter } from 'vue-router';
import { onMounted, watch } from 'vue';
import { computed } from 'vue';

const ty2 = ref();

const selectedTopic = ref();
const selectedIssue = ref();
const reTopic = ref();
const reIssue = ref();
const selectedLevel = ref();
const detail = ref();
const selectedDateStart = ref('');
const selectedDateEnd = ref('');
const meme = ref()
const filteredItems = ref([])
const showFullDetailModal_S = ref(false);
const selectedService_S = ref({});

const showFullDetailModal = ref(false);
const selectedService = ref({});
const tableData = ref([]);
const currentDataIndex = ref(null);
const searchQuery = ref('');

const currentPage = ref(1);
const perPage = ref(10);

const paginatedData = computed(() => {
  const start = (currentPage.value - 1) * perPage.value;
  const end = start + perPage.value;
  return filteredItems.value.slice(start, end);
});

const totalPages = computed(() => {
  return Math.ceil(filteredItems.value.length / perPage.value);
});

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value += 1;
  }
};

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value -= 1;
  }
};

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

const test = () => {
  console.log(selectedDateStart.value)
  console.log(selectedDateEnd.value)
}

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


const Search_Show = async () => {
  try {
    const response = await $fetch('http://172.29.10.238:8080/api/IT_Report_TableController', {
      method: 'POST',
      body: {
        report_by: selectedTopic.value,
        group_by: selectedIssue.value,
        start_date: selectedDateStart.value,
        end_date: selectedDateEnd.value

      },
      credentials: 'include',
    });
    const data = response.results
    meme.value = response.results;
    tableData.value = response.results;
    filteredItems.value = meme.value;
    console.log(selectedDateStart.value, selectedDateEnd.value, selectedTopic.value, selectedIssue.value)
  } catch (error) {
    console.error('Failed to fetch data:', error);
  }
};
const applySearch = () => {
  filteredItems.value = meme.value.filter(item => {
    return Object.values(item).some(value =>
      String(value).toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  });
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

const showFullDetail_S = (item) => {
  selectedService_S.value = { ...item };
  showFullDetailModal_S.value = true;
};

const showFullDetail = (item) => {
  selectedService.value = item;
  currentDataIndex.value = tableData.value.findIndex(ite => ite === item);
  showFullDetailModal.value = true;
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

.download-button {
  padding: 5px 10px;
  background-color: #f472b6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;

}

/* */
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
  width: 100px;
}


.detail-column {
  width: 100px;
}


.level-column {
  width: 100px;
}

.technician-column {
  width: 250px;
}

.due_by-column {
  width: 170px;
}

.detail-admin-column {
  width: 250px;
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

/*  */
.search-pagination-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.search-bar {
  display: flex;
  align-items: center;
}

.table-page {
  display: flex;
  justify-content: space-between;
  /* จัดให้อยู่ห่างจากกันด้วยช่องว่าง */
  align-items: center;
  /* จัดให้อยู่ตรงกลางในแนวตั้ง */
}

.table-page span {
  margin-left: right;
  /* ส่วนที่เหลือของ header จะจัดตำแหน่งไปทางขวาที่สุด */
}

.header button {
  margin-left: 0.2rem;
  /* กำหนดระยะห่างระหว่างปุ่ม */
}
</style>
