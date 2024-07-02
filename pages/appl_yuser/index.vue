<template>
  <div>
    <title>TAKAHATA</title>

    <!-- Box content with text -->
    <header class="box-content h-8 w-50 p-4 bg-slate-50">
      <div class="flex items-center justify-between">
        <a href="#" class="-m-1.5 p-1.5">
          <img class="h-8 w-auto" src="/image/ex.png" alt="" />
        </a>



        <div class="flex items-center space-x-2">
          <router-link to="/home_admin/">
            <button class="rounded-full bg-gray-300 text-gray-900 px-4 py-2">Home</button>
          </router-link>

          <div class="relative">
            <!-- Dropdown Menu Button -->
            <button @click="toggleDropdown1" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
              Menu
            </button>
            <div v-if="dropdownOpen1" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
              <!-- <router-link to="/appl_yuser/">
                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Applyuser</a>
              </router-link> -->
              <router-link to="/UserDataTable/">
                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">personal information</a>
              </router-link>
            </div>
          </div>

          <div class="relative">
            <!-- Dropdown Menu Button -->
            <button @click="toggleDropdown" class="rounded-full bg-gray-300 text-gray-900 px-4 py-2">
              Notification
            </button>
            <div v-if="dropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
              <router-link to="/Request_admin/">
                <a class="block px-4 py-2 text-gray-900 hover:bg-gray-200">Request</a>
              </router-link>
              <router-link to="/Services_admin/">
                <a class="block px-4 py-2 text-gray-900 hover:bg-gray-200">Services</a>
              </router-link>
              <router-link to="/Report/">
                <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Report</a>
              </router-link>
            </div>
          </div>
          <a href="https://sites.google.com/view/takahata" target="_blank">
            <button class="rounded-full bg-gray-300 text-gray-900 px-4 py-2">
              Shuttle booking
            </button>
          </a>
          <router-link to="/">
            <button class="rounded-full bg-green-600 text-white px-4 py-2">
              Logout
            </button>
          </router-link>
          <!-- Dropdown Menu Button -->
        </div>
      </div>
    </header>
    <div class="p-4 bg-sky-300">
      <div class="text-center">
        <h2 class="text-2xl font-semibold">กรอบรายชื่อพนักงาน</h2>
      </div>
      <main class="container mx-auto py-5">
        <div class="bg-yellow-100 mx-auto max-w-lg">
          <div class="border-b pb-12 mx-auto max-w-sm">
            <div class="grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-3">

              <div class="sm:col-span-1">
                <div class="mt-3">
                  <label for="TitleName" class="block text-sm font-medium leading-6 text-gray-900">TitleName</label>
                  <div class="mt-2">

                    <select id="currency" name="currency"
                      class="block w-full rounded-md  border-0 py-1.5 pl-3  text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                      v-model="selectedTitle" required>
                      <option>Mr.</option>
                      <option>Ms.</option>
                      <option>Mrs.</option>
                    </select>
                  </div>
                </div>
              </div>

              <div class="sm:col-span-1">
                <div class="mt-3">
                  <label for="region" class="block text-sm font-medium leading-6 text-gray-900">First name</label>
                  <div class="mt-2">
                    <input v-model="first_name" type="text" name="region" id="region" autocomplete="region"
                      class="block w-full rounded-md border-0 py-1.5 pl-3  text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                  </div>
                </div>
              </div>

              <div class="sm:col-span-1">
                <div class="mt-3">
                  <label for="postal-code" class="block text-sm font-medium leading-6 text-gray-900">Last name</label>
                  <div class="mt-2">
                    <input v-model="last_name" type="text" name="postal-code" id="postal-code"
                      autocomplete="postal-code"
                      class="block w-full rounded-md border-0 py-1.5 pl-3  text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                  </div>
                </div>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2">
                <label for="en" class="block text-sm font-medium leading-6 text-gray-900">En</label>
                <div class="mt-2">
                  <input v-model="en" type="text" name="en" id="en" autocomplete="en"
                    class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2">
                <label for="Job_position" class="block text-sm font-medium leading-6 text-gray-900">Job position</label>
                <div class="mt-2">
                  <select id="Job_position" name="Job_position" autocomplete="Job_position"
                    class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                    v-model="role" required>
                    <option>Admin(0)</option>
                    <option>Employee(1)</option>
                    <option>Manager3(2)</option>
                    <option>Manager2(3)</option>
                    <option>Manager1(4)</option>
                    <option>Vice President(5)</option>
                  </select>
                </div>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="Section" class="block text-sm font-medium leading-6 text-gray-900">Section</label>
              <div class="mt-2">
                <select id="Section" name="Section" autocomplete="Section"
                  class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                  v-model="section" required>
                  <option>ACC</option>
                  <option>CRC</option>
                  <option>HRD</option>
                  <option>BOL</option>
                  <option>PUS</option>
                  <option>DCC</option>
                  <option>CTS</option>
                  <option>PLN</option>
                  <option>STA</option>
                  <option>QAT</option>
                  <option>REC</option>
                  <option>NMD</option>
                  <option>PRD</option>
                  <option>PMF</option>
                  <option>MMT</option>
                  <option>TEC</option>
                  <option>WHC</option>
                  <option>ASY</option>
                  <option>admin</option>
                  <option>Vice President</option>



                </select>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="branch" class="block text-sm font-medium leading-6 text-gray-900">branch</label>
              <div class="mt-2">
                <input v-model="branch" type="text" name="branch" id="branch" autocomplete="branch"
                  class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="tel" class="block text-sm font-medium leading-6 text-gray-900">Tel</label>
              <div class="mt-2">
                <input v-model="tel" type="text" name="tel" id="tel" autocomplete="tel"
                  class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="user-name" class="block text-sm font-medium leading-6 text-gray-900">User</label>
              <div class="mt-2">
                <input v-model="user" type="text" name="user-name" id="user-name" autocomplete="user-name"
                  class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">password</label>
              <div class="mt-2">
                <input v-model="password" id="password" name="password" type="password" autocomplete="current-password"
                  required
                  class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="mt-4 sm:col-span-2">
              <div class="mt-2">
                <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Email address</label>
                <div class="mt-2">
                  <input v-model="email" id="email" name="email" type="email" autocomplete="email"
                    class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
            </div>

            <div class="mt-4 sm:col-span-2">
              <div class="mt-2">
                <label for="email" class="block text-sm font-medium leading-6 text-gray-900">หากไม่มีข้อมูลให้ใส่ - แทน</label>
                <div class="mt-2">
                 
                </div>
              </div>
            </div>

           
            

            <div class="mt-10">
              <div class="flex items-center justify-center space-x-2 flex-grow">
                <button @click="showModal = true" class="rounded-full bg-green-600 text-white px-4 py-2">Submit</button>
                <router-link to="/home_admin/">
                  <button class="rounded-full bg-red-700 text-white px-4 py-2">Cancel</button>
                </router-link>
                <!-- <button @click="test" class="rounded-full bg-red-700 text-white px-4 py-2">test</button> -->

              </div>
              <!--  -->
              <div v-if="showModal"
                class="fixed inset-0 flex items-center justify-center z-50 bg-gray-800 bg-opacity-75">
                <div class="bg-white p-6 rounded-lg shadow-lg">
                  <h3 class="text-lg font-semibold mb-4">Confirm Submission</h3>
                  <p class="mb-4">Are you sure you want to submit this form?</p>
                  <div class="flex justify-end space-x-2">
                    <button @click="handleSubmit" class="rounded-full bg-green-600 text-white px-4 py-2">YES</button>
                    <button @click="showModal = false"
                      class="rounded-full bg-red-700 text-white px-4 py-2">Cancel</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>
      <!-- ตาราง -->

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { jwtDecode } from 'jwt-decode';
import bcrypt from 'bcryptjs';

const token = jwtDecode(useCookie("token").value);

const showModal = ref(false);
const dropdownOpen = ref(false);
const dropdownOpen1 = ref(false);

//variable collect data
const selectedTitle = ref()
const first_name = ref();
const last_name = ref();
const en = ref();
const role = ref();
const section = ref();
const branch = ref();
const tel = ref();
const user = ref();
const password = ref();
const email = ref();


console.log(selectedTitle.value)
const handleSubmit = async () => {
  showModal.value = false; // Close the modal
  await saveData(); // Submit the form data
  navigateTo('/home_admin/');
  // Optionally, navigate to another page or show a success message
};

const saveData = async () => {
  try {
    const salt = await bcrypt.genSalt(12);
    const hashedPass = await bcrypt.hash(password.value, salt)

    const response = await $fetch('http://172.29.10.238:8080/api/register', {
      method: "POST",
      body: {

        title: selectedTitle.value,
        name: first_name.value,
        surname: last_name.value,
        en: en.value,
        role: role.value,
        section: section.value,
        branch: branch.value,
        tel: tel.value,
        user: user.value,
        password: hashedPass,
        email: email.value
      },
      credentials: 'include'
    });

  } catch (error) {
    console.error('Failed to save data:', error);
  }
}

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value;
}
function toggleDropdown1() {
  dropdownOpen1.value = !dropdownOpen1.value;
}
</script>

<style scoped>
/* Existing styles */
</style>
