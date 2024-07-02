<template>
  <div class="overflow-hidden">
    <title>TAKAHATA</title>

    <!-- Box content with text -->
    <header class="box-content h-8 w-50 p-4 bg-slate-200">
      <div class="flex items-center justify-between">
        <a href="#" class="-m-1.5 p-1.5">
          <img class="h-8 w-auto" src="/image/ex.png" alt="" />
        </a>

        <div>
          <!-- Name -->
          <div class="text-center mt-4">
            <p class="text-lg font-semibold" style="margin-left: auto; margin-left: 100px;">
              {{ token.name }}
            </p>
          </div>
        </div>

        <div class="flex items-center space-x-2">
          <router-link to="/home_admin/">
            <button class="rounded-full bg-gray-300 text-gray-900 px-4 py-2">Home</button>
          </router-link>
          <div class="relative">
            <!-- Dropdown Menu Button -->
            <button @click="toggleDropdown" class="rounded-full bg-gray-300 text-gray-900 px-4 py-2">
              Notification
            </button>
            <div v-if="dropdownOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg">
              <router-link to="/Notificationr/">
                <a class="block px-4 py-2 text-gray-900 hover:bg-gray-200">Request</a>
              </router-link>
              <router-link to="/Notifications/">
                <a class="block px-4 py-2 text-gray-900 hover:bg-gray-200">Services</a>
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
                  <label for="city" class="block text-sm font-medium leading-6 text-gray-900">TitleName</label>
                  <div class="mt-2">
                    <select id="TitleName" name="TitleName" autocomplete="TitleName"
                      class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                      <option>United States</option>
                      <option>Mr.</option>
                      <option>Ms.</option>
                    </select>
                  </div>
                </div>
              </div>

              <div class="sm:col-span-1">
                <div class="mt-3">
                  <label for="region" class="block text-sm font-medium leading-6 text-gray-900">First name</label>
                  <div class="mt-2">
                    <input type="text" name="region" id="region" autocomplete="address-level1"
                      class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                  </div>
                </div>
              </div>

              <div class="sm:col-span-1">
                <div class="mt-3">
                  <label for="postal-code" class="block text-sm font-medium leading-6 text-gray-900">Last name</label>
                  <div class="mt-2">
                    <input type="text" name="postal-code" id="postal-code" autocomplete="postal-code"
                      class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                  </div>
                </div>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2">
                <label for="first-name" class="block text-sm font-medium leading-6 text-gray-900">En</label>
                <div class="mt-2">
                  <input type="text" name="first-name" id="first-name" autocomplete="given-name"
                    class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2">
                <label for="first-name" class="block text-sm font-medium leading-6 text-gray-900">Job position</label>
                <div class="mt-2">
                  <input type="text" name="first-name" id="first-name" autocomplete="given-name"
                    class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">Section</label>
              <div class="mt-2">
                <input type="text" name="last-name" id="last-name" autocomplete="family-name"
                  class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="first-name" class="block text-sm font-medium leading-6 text-gray-900">branch</label>
              <div class="mt-2">
                <input type="text" name="first-name" id="first-name" autocomplete="given-name"
                  class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">Tel</label>
              <div class="mt-2">
                <input type="text" name="last-name" id="last-name" autocomplete="family-name"
                  class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">User</label>
              <div class="mt-2">
                <input type="text" name="last-name" id="last-name" autocomplete="family-name"
                  class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="sm:col-span-1">
              <div class="mt-2"></div>
              <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">password</label>
              <div class="mt-2">
                <input type="text" name="last-name" id="last-name" autocomplete="family-name"
                  class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
              </div>
            </div>

            <div class="mt-4 sm:col-span-2">
              <div class="mt-2">
                <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Email address</label>
                <div class="mt-2">
                  <input id="email" name="email" type="email" autocomplete="email"
                    class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
            </div>
            <div class="mt-10">
              <div class="flex items-center justify-center space-x-2 flex-grow">
                <button @click="showModal = true" class="rounded-full bg-green-600 text-white px-4 py-2">Submit</button>
                <router-link to="/home/">
                  <button class="rounded-full bg-red-700 text-white px-4 py-2">Cancel</button>
                </router-link>
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

const token = jwtDecode(useCookie("token").value);

const showModal = ref(false);
const dropdownOpen = ref(false);

const handleSubmit = async () => {
  showModal.value = false; // Close the modal
  navigateTo('/home/');
  await saveData(); // Submit the form data

  // Optionally, navigate to another page or show a success message
};

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value;
}
</script>

<style scoped>
.overflow-hidden {
  overflow-y: hidden;
}
</style>
