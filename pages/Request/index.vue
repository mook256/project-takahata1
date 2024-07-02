<template>
  <title>TAKAHATA</title>
  <div>
    <!-- Header -->
    <header class="box-content h-8 w-50 p-4 bg-Slate-50">
      <div class="flex items-center justify-between">
        <a href="#" class="-m-1.5 p-1.5">
          <img class="h-8 w-auto" src="/image/ex.png" alt="" />
        </a>
        <div class="flex items-center space-x-2">
          <router-link to="">
            <button @click="gotoHome(token.role)" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">Home</button>
          </router-link>

          <div class="relative">
            <!-- Dropdown Menu Button -->
            <button @click="toggleDropdown" class="rounded-full bg-gray-200 text-gray-800 px-4 py-2">
              Notification
            </button>
            <div v-if="dropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
              <router-link to="">
                <a @click="gotRequest(token.role)" class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Request</a>
              </router-link>
              <router-link to="">
                <a @click="gotoServices(token.role)"
                  class="block px-4 py-2 text-gray-800 hover:bg-gray-200">Services</a>
              </router-link>
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

    <!-- Main Content -->
    <div class="bg-sky-300">
      <main class="container mx-auto py-5 bg-Teal-600 w-50 h-50">
        <div class="text-center mb-8">
          <h2 class="text-2xl font-semibold">ระบบใบคำร้องขอ</h2>
        </div>
        <div class="container mx-auto max-w-lg rounded-lg bg-sky-50">
          <!-- Column 1 -->
          <div class="border rounded-md p-4 mb-4">
            <fieldset>
              <legend class="text-sm font-semibold leading-6 text-gray-900">User Account</legend>
              <div class="mt-6 space-y-6">
                <div class="flex items-center gap-x-3">
                  <input id="push-everything0" name="push-everything0" type="checkbox"
                    class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                    @change="handleCheckboxChange" />
                  <label for="push-everything0" class="block text-sm font-medium leading-6 text-gray-900">User
                    Login</label>
                </div>
                <div class="flex items-center gap-x-3">
                  <input id="push-everything1" name="push-everything1" type="checkbox"
                    class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                    @change="handleCheckboxChange" />
                  <label for="push-everything1" class="block text-sm font-medium leading-6 text-gray-900">User
                    E-mail</label>
                </div>
                <div class="flex items-center gap-x-3">
                  <input id="push-everything2" name="push-everything2" type="checkbox"
                    class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                    @change="handleCheckboxChange" />
                  <label for="push-everything2" class="block text-sm font-medium leading-6 text-gray-900">User
                    Internet</label>
                </div>
                <div class="flex items-center gap-x-3">
                  <input id="push-everything3" name="push-everything3" type="checkbox"
                    class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                    @change="handleCheckboxChange" />
                  <label for="push-everything3"
                    class="block text-sm font-medium leading-6 text-gray-900">UserERP&MRP</label>
                </div>

                <div>
                  <label for="userReq" class="block text-sm font-medium leading-6 text-gray-900">Title/name</label>
                  <div class="relative mt-2 rounded-md shadow-sm flex">
                    <select id="currency" name="currency"
                      class="h-full rounded-l-md border-0 bg-transparent py-0 pr-7 text-gray-500 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm"
                      v-model="selectedTitle" required>
                      <option>Mr.</option>
                      <option>Ms.</option>
                      <option>Mrs.</option>
                    </select>
                    <input type="text" name="userReq" id="userReq"
                      class="block w-full rounded-r-md border-0 py-1.5 pl-7 pr-12 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                      rows="3" v-model="userReq" required>
                  </div>
                </div>
              </div>
            </fieldset>

            <!-- Column 2 -->

            <div class="mt-10 grid grid-cols-1 divide-y">
              <fieldset>
                <legend class="text-sm font-semibold leading-6 text-gray-900">Hardware</legend>
                <div class="mt-6 space-y-6">
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything4" name="push-everything4" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything4" class="block text-sm font-medium leading-6 text-gray-900">Desktop
                      PC</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything5" name="push-everything5" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything5"
                      class="block text-sm font-medium leading-6 text-gray-900">Notebook</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything6" name="push-everything6" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything6"
                      class="block text-sm font-medium leading-6 text-gray-900">Printer</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything7" name="push-everything7" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything7" class="block text-sm font-medium leading-6 text-gray-900">Barcode
                      Scanner</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything8" name="push-everything8" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything8" class="block text-sm font-medium leading-6 text-gray-900">CCTV</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything9" name="push-everything9" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything9"
                      class="block text-sm font-medium leading-6 text-gray-900">Smartphone&Tablet</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything10" name="push-everything10" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything10"
                      class="block text-sm font-medium leading-6 text-gray-900">Other</label>
                  </div>
                </div>
              </fieldset>
            </div>
            <!-- Column 3 -->
            <div class="mt-10 grid grid-cols-1 divide-y ">
              <fieldset>
                <legend class="text-sm font-semibold leading-6 text-gray-900">Software License</legend>
                <div class="mt-6 space-y-6">
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything11" name="push-everything11" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything11" class="block text-sm font-medium leading-6 text-gray-900">Microsoft
                      Office</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything12" name="push-everything12" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything12"
                      class="block text-sm font-medium leading-6 text-gray-900">AutoCAD</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything13" name="push-everything13" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything13" class="block text-sm font-medium leading-6 text-gray-900">Jancom
                      (ACC)</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything14" name="push-everything14" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything14" class="block text-sm font-medium leading-6 text-gray-900">Tiger HER
                      (HRD)</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything15" name="push-everything15" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything15"
                      class="block text-sm font-medium leading-6 text-gray-900">Other</label>
                  </div>
                </div>
              </fieldset>
            </div>
            <!-- Column 4-->
            <div class="mt-10 grid grid-cols-1 divide-y ">
              <fieldset>
                <legend class="text-sm font-semibold leading-6 text-gray-900">ERP&MRP</legend>
                <div class="mt-6 space-y-6">
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything16" name="push-everything16" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything16"
                      class="block text-sm font-medium leading-6 text-gray-900">Real-Time</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything17" name="push-everything17" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything17" class="block text-sm font-medium leading-6 text-gray-900">DCS</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything18" name="push-everything18" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything18"
                      class="block text-sm font-medium leading-6 text-gray-900">FOXPRO</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything19" name="push-everything19" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything19" class="block text-sm font-medium leading-6 text-gray-900">SAP</label>
                  </div>
                </div>
              </fieldset>
            </div>
            <!-- Column 5 -->
            <div class="mt-10 grid grid-cols-1 divide-y ">
              <fieldset>
                <legend class="text-sm font-semibold leading-6 text-gray-900">File Server</legend>
                <div class="mt-6 space-y-6">
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything20" name="push-everything20" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything20"
                      class="block text-sm font-medium leading-6 text-gray-900">Folder</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything21" name="push-everything21" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything21"
                      class="block text-sm font-medium leading-6 text-gray-900">Authentication</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything22" name="push-everything22" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything22"
                      class="block text-sm font-medium leading-6 text-gray-900">Quota</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything23" name="push-everything23" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything23" class="block text-sm font-medium leading-6 text-gray-900">Restore
                      Data</label>
                  </div>

                  <div class="sm:col-span-1">
                    <label for="file_name"
                      class="block text-sm font-medium leading-6 text-gray-900">โปรดระบุไฟล์</label>
                    <div class="mt-2">
                      <input type="text" name="file_name" id="file_name" autocomplete="given-name"
                        class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                        rows="3" v-model="file_name" required>
                    </div>
                  </div>

                </div>
              </fieldset>
            </div>
            <!-- Column 6-->
            <div class="mt-10 grid grid-cols-1 divide-y ">
              <fieldset>
                <legend class="text-sm font-semibold leading-6 text-gray-900">Installation </legend>
                <div class="mt-6 space-y-6">
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything24" name="push-everything24" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything24" class="block text-sm font-medium leading-6 text-gray-900">Wire
                      Network (LAN)</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything25" name="push-everything25" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything25" class="block text-sm font-medium leading-6 text-gray-900">Wireless
                      (WiFi)</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything26" name="push-everything26" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything26"
                      class="block text-sm font-medium leading-6 text-gray-900">Internet</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything27" name="push-everything27" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything27" class="block text-sm font-medium leading-6 text-gray-900">VPN</label>
                  </div>
                  <div class="flex items-center gap-x-3">
                    <input id="push-everything28" name="push-everything28" type="checkbox"
                      class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
                      @change="handleCheckboxChange" />
                    <label for="push-everything28"
                      class="block text-sm font-medium leading-6 text-gray-900">Telephone</label>
                  </div>
                </div>
              </fieldset>
            </div>
            <div class="mt-10 grid grid-cols-1 divide-y ">
              <fieldset>
                <legend class="text-sm font-semibold leading-6 text-gray-900">Level</legend>
                <div class="mt-6 space-y-6">
                  <select id="currency" name="currency"
                    class="h-full rounded-md border-0 bg-transparent py-0 pl-2 pr-7 text-gray-900 sm:text-sm outline outline-offset-2 outline-white-500"
                    v-model="selectedLevel" required>
                    <option>Not Specified</option>
                    <option>High</option>
                    <option>Low</option>
                    <option>Medium</option>
                    <option>Normal</option>
                  </select>
                </div>
              </fieldset>
            </div>

          </div>









          <!-- Explanation -->
          <div class="mt-6">
            <div class="text-center mb-8">
              <h2 class="text-2xl font-semibold">Explanation</h2>
            </div>
            <div class="container mx-auto max-w-md rounded-lg bg-sky-50">
              <textarea id="explanation"
                class="form-control mx-auto h-20 w-full border rounded-lg px-4 outline-none focus:ring focus:ring-indigo-600"
                rows="10" v-model="explanation" required></textarea>
            </div>
            <!-- Error Message -->
            <div v-if="explanationError" class="text-red-500 text-sm mt-2">{{ explanationError }}</div>
          </div>
          <!-- Buttons -->
          <div class="mt-6">
            <div class="flex items-center justify-center space-x-2">

            </div>
          </div>
          <!-- Buttons -->
          <div class="flex items-center justify-center space-x-2 flex-grow">
            <button @click="showModal = true" class="rounded-full bg-green-500 text-white px-4 py-2">Submit</button>
            <button @click="gotoHome(token.role)" class="rounded-full bg-red-600 text-white px-4 py-2">Cancel</button>

          </div>

          <!-- Modal -->
          <div v-if="showModal" class="fixed inset-0 flex items-center justify-center z-50 bg-gray-800 bg-opacity-75">
            <div class="bg-white p-6 rounded-lg shadow-lg">
              <h3 class="text-lg font-semibold mb-4">Confirm Submission</h3>
              <p class="mb-4">Are you sure you want to submit this form?</p>
              <div class="flex justify-end space-x-2">
                <button @click="handleSubmit" class="rounded-full bg-green-500 text-white px-4 py-2">YES</button>
                <button @click="showModal = false" class="rounded-full bg-red-600 text-white px-4 py-2">Cancel</button>
              </div>
            </div>
          </div>

          <!-- Buttons -->
          <div class="mt-6">
            <div class="flex items-center justify-center space-x-2">

            </div>
          </div>

          <div class="flex items-center justify-center space-x-2 flex-grow mt-6"></div>
        </div>
      </main>
      <div></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { jwtDecode } from "jwt-decode";

const ty1 = ref([]);
const token = jwtDecode(useCookie("token").value); // nuxt cookie & jwtDecode nuxt
const selectedTitle = ref();
const selectedLevel = ref();
const userReq = ref('');
const file_name = ref('');
const explanation = ref('');
const explanationError = ref('');
const showModal = ref(false);
const show = ref()
const arr = Array.from(Array(32), () => 0)
const dropdownOpen = ref(false);

const submitbutton = async () => {
  try {
    const response = await $fetch('http://172.29.10.238:8080/api/IT_Request_Submit', {
      method: "POST",
      body: {
        en: token.userId,
        name: token.name,
        section_id: token.department,
        branch: token.branch,
        tel: token.tel,
        selected_choices: arr,
        detail_user: selectedTitle.value + ' ' + userReq.value,
        detail_file: file_name.value,
        detail: explanation.value,
        status: "Open",
        level: selectedLevel.value,
        role: token.role,
      },
      credentials: 'include'
    });

  } catch (error) {
    console.error('Failed to save data:', error);
  }
}

const fetchData = async () => {
  try {
    const response = await $fetch('http://172.29.10.238:8080/api/get/typeRepair', { credentials: 'include' });
    ty1.value = response.results;
  } catch (error) {
    console.error('Failed to fetch Topic type data:', error);
  }
};

const handleSubmit = async () => {
  explanationError.value = ''; // Reset error message
  if (!explanation.value.trim()) {
    explanationError.value = 'Explanation is required';
    return; // Stop form submission
  }
  showModal.value = false; // Close the modal
  arr[29] = selectedTitle.value + ' ' + userReq.value;
  arr[30] = file_name.value;
  arr[31] = explanation.value;
  submitbutton();
  gotoHome(token.role);
//  gotoServices(token.role);
  //gotRequest(token.role);
};

function gotoHome(checkRole) {
  if (checkRole == "0") {
    navigateTo('/home_admin/');
  } else if (checkRole == "1") {
    navigateTo('/home/');
  }
  else {
    navigateTo('/home_manager/');
  }
}

function gotoServices(checkRole1) {
  if (checkRole1 == "0") {
    navigateTo('/Services_admin/');
  } else if (checkRole1 == "1") {
    navigateTo('/Alert_Services_User/');
  }
  else {
    navigateTo('/Alert_Services_mangger/');
  }
}

function gotRequest(checkRole2) {
  if (checkRole2 == "0") {
    navigateTo('/Request_admin/');
  } else if (checkRole2 == "1") {
    navigateTo('/Alert_Request_User/');
  }
  else {
    navigateTo('/Alert_Request_manager/');
  }
}

const handleCheckboxChange = (event) => {
  const checkboxMapping = {
    'push-everything0': 0,
    'push-everything1': 1,
    'push-everything2': 2,
    'push-everything3': 3,
    'push-everything4': 4,
    'push-everything5': 5,
    'push-everything6': 6,
    'push-everything7': 7,
    'push-everything8': 8,
    'push-everything9': 9,
    'push-everything10': 10,
    'push-everything11': 11,
    'push-everything12': 12,
    'push-everything13': 13,
    'push-everything14': 14,
    'push-everything15': 15,
    'push-everything16': 16,
    'push-everything17': 17,
    'push-everything18': 18,
    'push-everything19': 19,
    'push-everything20': 20,
    'push-everything21': 21,
    'push-everything22': 22,
    'push-everything23': 23,
    'push-everything24': 24,
    'push-everything25': 25,
    'push-everything26': 26,
    'push-everything27': 27,
    'push-everything28': 28
  };

  const index = checkboxMapping[event.target.id];
  if (index !== undefined) {
    arr[index] = arr[index] === 0 ? index + 1 : 0;
  }
  fetchData();
};

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value;
}

</script>
