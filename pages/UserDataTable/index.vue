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

          <router-link to="/Alert_Services_User/">
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
      <h2 class="text-2xl font-semibold">User Data</h2>
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
            <th class="en-column">EN</th>
            <th class="Title-column">Title</th>
            <th class="Firtst-name-column">First name</th>
            <th class="Last-name-column">Last name</th>
            <th class="branch-column">Branch</th>
            <th class="tel-column">Tel</th>
            <th class="user-column">User</th>
            <th class="email-address-column">email</th>
            <th class="department-column">department</th>
            <th class="edit-information-column">Edit information</th>
            <th class="delete-data-column">Delete data</th>

          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr v-for="(item, index) in  displayedItems" :key="index">
            <td class="en-column">
              <template v-if="editIndex !== index">
                {{ item.en }}
              </template>
              <template v-else>
                <input v-model="editValues.en" class="edit-input en-column" />
              </template>
            </td>
            <td class="Title-column">
              <template v-if="editIndex !== index">
                {{ item.title_name }}
              </template>
              <template v-else>
                <input v-model="editValues.title_name" class="edit-input Title-column" />
              </template>
            </td>
            <td class="Firtst-name-column">
              <template v-if="editIndex !== index">
                {{ item.name }}
              </template>
              <template v-else>
                <input v-model="editValues.name" class="edit-input Firtst-name-column" />
              </template>
            </td>
            <td class="Last-name-column">
              <template v-if="editIndex !== index">
                {{ item.last_name }}
              </template>
              <template v-else>
                <input v-model="editValues.last_name" class="edit-input Last-name-column" />
              </template>
            </td>
            <td class="branch-column">
              <template v-if="editIndex !== index">
                {{ item.branch }}
              </template>
              <template v-else>
                <input v-model="editValues.branch" class="edit-input branch-column" />
              </template>
            </td>

            <td class="tel-column">
              <template v-if="editIndex !== index">
                {{ item.tel }}
              </template>
              <template v-else>
                <input v-model="editValues.tel" class="edit-input tel-column" />
              </template>
            </td>
            <td class="user-column">
              <template v-if="editIndex !== index">
                {{ item.user }}
              </template>
              <template v-else>
                <input v-model="editValues.user" class="edit-input user-column" />
              </template>
            </td>
            <td class="email-address-column">
              <template v-if="editIndex !== index">
                {{ item.e_mail }}
              </template>
              <template v-else>
                <input v-model="editValues.e_mail" class="edit-input email-address-column" />
              </template>
            </td>

            <td class="department-column">
              <template v-if="editIndex !== index">
                {{ item.department_id }}
              </template>
              <template v-else>
                <input v-model="editValues.department_id" class="edit-input department-column" />
              </template>
            </td>



            <td class="edit-information-column">
              <template v-if="editIndex !== index">
                <button @click="startEdit(index)" class="edit-button">Edit</button>
              </template>
              <template v-else>
                <button @click="applyEdit(index)" class="apply-button">Apply</button>
                <button @click="cancelEdit" class="cancel-button">Cancel</button>
              </template>
            </td>
            <td class="delete-data-column">
              <button @click="deleteEmployee(index)" class="edit-button">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Popup Modal for Details -->

    <!-- Popup Modal for Admin Detail -->

    <!-- Popup Modal for Editing Manager Note -->
  </div>
</template>

<script setup>
import { ref } from 'vue';

const meme = ref([]);

const searchQuery = ref('');
const filteredItems = ref([]);
const editIndex = ref(null);
const editValues = ref({
  en: '',
  requester: '',
  section: '',
  branch: '',
  tel: '',
  user: '',
  email: ''
});

const Show_On_Table = async () => {
  try {
    const response = await fetch('http://172.29.10.238:8080/api/EmployeeTable', {
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

const update_del_data = async (en_key, new_key, title, first_name, last_name, branch, tel, user, email, section, mode) => {
  try {
    await fetch('http://172.29.10.238:8080/api/EmployeeUpdateController', {
      method: 'POST',
      body: JSON.stringify({
        old_en: en_key,
        en: new_key,
        title: title,
        name: first_name,
        surname: last_name,
        branch: branch,
        tel: tel,
        user: user,
        email: email,
        section: section,
        mode: mode
  
      }),
      headers: {
        'Content-Type': 'application/json',
      },
      credentials: 'include',
    });
    console.log("Fetched Success")
  } catch (error) {
    console.error('Failed to fetch data:', error);
  }
};

const editEmployee = (index) => {
  const employee = filteredItems.value[index];
  // Additional logic for editing employee...
};

const deleteEmployee = async (index) => {
  const isConfirmed = confirm("Are you sure you want to delete this employee?");
  if (isConfirmed) {
    const updatedItem = editValues.value;
    const old_EN = filteredItems.value[index].en;
    filteredItems.value.splice(index, 1);
    try {
      await update_del_data(old_EN, updatedItem["en"], updatedItem["title_name"], updatedItem["name"], updatedItem["last_name"], updatedItem["branch"], updatedItem["tel"], updatedItem["user"], updatedItem["e_mail"], updatedItem["department_id"], 1);
      window.location.reload(); // Reload the page after successful deletion
    } catch (error) {
      console.error('Failed to delete data:', error);
    }
  }
 
};


const applySearch = () => {
  filteredItems.value = meme.value.filter(item => {
    return Object.values(item).some(value =>
      String(value).toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  });
};

const startEdit = (index) => {
  editIndex.value = index;
  editValues.value = { ...filteredItems.value[index] };
};

const applyEdit = async (index) => {
  const updatedItem = editValues.value;
  const old_EN = filteredItems.value[index].en;
  console.log(old_EN)
  await update_del_data(old_EN,updatedItem["en"],updatedItem["title_name"],updatedItem["name"],updatedItem["last_name"],updatedItem["branch"],updatedItem["tel"],updatedItem["user"],updatedItem["e_mail"],updatedItem["department_id"],0)

  try {
   for (const field in updatedItem) {
      console.log(field)
      console.log(updatedItem[field])
      //await update_del_data(old_EN, field, updatedItem[field], 'update');
    }

    filteredItems.value[index] = { ...updatedItem };
    editIndex.value = null;
  } catch (error) {
    console.error('Failed to update data:', error);
  }
};

const cancelEdit = () => {
  editIndex.value = null;
  editValues.value = {};
};

const mapSection = {
  "ACC": 1,
  "CRC": 2,
  "HRD": 3,
  "BOL": 4,
  "PUS": 5,
  "DCC": 6,
  "CTS": 7,
  "PLN": 8,
  "STA": 9,
  "QAT": 10,
  "REC": 11,
  "NMD": 12,
  "PRD": 13,
  "PMF": 14,
  "MMT": 15,
  "TEC": 16,
  "WHC": 17,
  "ASY": 18,
  "admin": 19,
  "Vice President": 20,
};

function mapInputSection(input) {
  return mapSection[input] || null; // ถ้าไม่พบ mapping ให้คืนค่า null
}


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

.apply-button {
  background-color: green;
  color: white;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
}

.cancel-button {
  background-color: red;
  color: white;
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
  background-color: #f8f8f8;
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

.Title-column {
  width: 100px;
}

.Firtst-name-column {
  width: 150px;
}

.branch-column {
  width: 150px;
}

.tel-column {
  width: 90px;
}

.user-column {
  width: 170px;
}

.email-address-column {
  width: 250px;
  /* Adjusted width */
}

.edit-information-column {
  width: 250px;
}


.delete-data-column {
  width: 250px;
}

.due-by-column {
  width: 150px;
}

.user-column {
  width: 150px;
  /* Adjusted width */
}

.Last-name-column {
  width: 100px;
}

.department-column {
  width: 120px;
}

/* Input width adjustment */
.edit-input {
  width: 100%;
  /* Adjust input width to fit the column */
  box-sizing: border-box;
  /* Include padding and border in the element's total width and height */
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
  margin-bottom: 10px;
}

.modal-content ul {
  margin: 0;
  padding: 0;
  list-style-type: none;
  text-align: left;
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
  background-color: green;
  color: white;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
  margin-right: 10px;
  /* เพิ่มระยะห่างทางขวา */
}

.cancel-button {
  background-color: red;
  color: white;
  padding: 5px 10px;
  border: none;
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
