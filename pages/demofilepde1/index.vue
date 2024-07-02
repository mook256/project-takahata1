<template>
    <div>
     
        <button @click="downloadPDF">Download PDF</button>
    </div>
</template>

<script setup>
// import { ref } from 'vue';
import { jsPDF } from "jspdf";
import { font } from "./THSarabun-normal";

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

const downloadPDF = () => {
    const doc = new jsPDF();

    // Add custom font
    doc.addFileToVFS("MyFont.ttf", font);
    doc.addFont("MyFont.ttf", "MyFont", "normal");
    doc.setFont("MyFont");

    // Text strings
    doc.text("Hello world! กราบสวัสดีพ่อแม่", 10, 10);


    doc.save("test.pdf");
};
</script>
