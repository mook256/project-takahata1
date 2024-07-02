<template>
    <div>
      <h1>New Page</h1>
      <button @click="clearSavedDate">Go to Calendar Page</button>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const clearSavedDate = () => {
    localStorage.removeItem('selectedDateStart');
    sessionStorage.removeItem('selectedDateStart');

    localStorage.removeItem('selectedDateEnd');
    sessionStorage.removeItem('selectedDateEnd');

    localStorage.removeItem('selectedTopic');
    sessionStorage.removeItem('selectedTopic');

    localStorage.removeItem('selectedIssue');
    sessionStorage.removeItem('selectedIssue');

    localStorage.removeItem('reTopic');
    sessionStorage.removeItem('reTopic');

    localStorage.removeItem('reIssue');
    sessionStorage.removeItem('reIssue');
    window.location.href = '/demo5'; // Redirect to the calendar page
  };
  </script>
  
  <style>
  /* Add any custom styles for your new page here */
  </style>
  