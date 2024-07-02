<template>
    <div id="app">
        <div>
            <div>
                <h1 class="header">
                    <span></span>
                    <span>
                        Page {{ currentPage }} of {{ totalPages }}
                        <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
                        <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
                    </span>
                </h1>
            </div>

        </div>
        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in paginatedData" :key="item.id">
                    <td>{{ item.id }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.email }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const items = ref([
    { id: 1, name: 'John Doe', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
    { id: 3, name: 'Mike Johnson', email: 'mike@example.com' },
    { id: 4, name: 'Sara Wilson', email: 'sara@example.com' },
    { id: 5, name: 'Paul Brown', email: 'paul@example.com' },
    { id: 6, name: 'Emma Green', email: 'emma@example.com' },
    { id: 7, name: 'George Adams', email: 'george@example.com' },
    { id: 8, name: 'Olivia Lee', email: 'olivia@example.com' },
    { id: 9, name: 'James White', email: 'james@example.com' },
    { id: 10, name: 'Liam Harris', email: 'liam@example.com' }
]);

const currentPage = ref(1);
const perPage = ref(6);

const paginatedData = computed(() => {
    const start = (currentPage.value - 1) * perPage.value;
    const end = start + perPage.value;
    return items.value.slice(start, end);
});

const totalPages = computed(() => {
    return Math.ceil(items.value.length / perPage.value);
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
</script>

<style scoped>
table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 1rem;
}

th,
td {
    padding: 0.5rem;
    border: 1px solid #ddd;
}

button {
    margin: 0.3 0.2rem;
    padding: 0.5rem 1rem;
}
.header {
    display: flex;
    justify-content: space-between; /* จัดให้อยู่ห่างจากกันด้วยช่องว่าง */
    align-items: center; /* จัดให้อยู่ตรงกลางในแนวตั้ง */
}

.header span {
    margin-left: right; /* ส่วนที่เหลือของ header จะจัดตำแหน่งไปทางขวาที่สุด */
}

.header button {
    margin-left: 0.2rem; /* กำหนดระยะห่างระหว่างปุ่ม */
}


</style>