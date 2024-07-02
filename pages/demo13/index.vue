<template>
    <button @click="refreshPage">Refresh Page</button>
  </template>
  
  <script setup>
  function refreshPage() {
    window.location.reload();
  }
  </script>
  