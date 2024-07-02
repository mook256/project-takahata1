<template>
    <div class="date-container">
      <label for="date">เลือกวันที่:</label>
      <input type="date" v-model="selectedDate">
      <button @click="refreshPage">Refresh</button>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, watch } from 'vue';
  
  const selectedDate = ref('');
  
  onMounted(() => {
    const savedDate = localStorage.getItem('selectedDate');
    if (savedDate) {
      selectedDate.value = savedDate;
    } else {
      // If the page is newly opened, clear sessionStorage
      sessionStorage.removeItem('selectedDate');
    }
  });
  
  watch(selectedDate, (newDate) => {
    if (newDate) {
      localStorage.setItem('selectedDate', newDate);
    } else {
      localStorage.removeItem('selectedDate');
    }
  });
  
  const refreshPage = () => {
    window.location.reload();
  };
  </script>
  
  <style>
  .date-container label {
    display: block;
    margin-bottom: 5px;
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
  
  .date-container button:hover {
    background-color: #0056b3;
  }
  </style>
  