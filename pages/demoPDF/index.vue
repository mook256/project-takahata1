<template>
  <div>
    <input type="text" v-model="name" placeholder="ชื่อ">
    <input type="text" v-model="surname" placeholder="นามสกุล">
    <input type="text" v-model="phoneNumber" placeholder="เบอร์โทร">

    <button @click="generatePDF">ดาวน์โหลด PDF</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import jsPDF from 'jspdf';

const name = ref('');
const surname = ref('');
const phoneNumber = ref('');

const generatePDF = () => {
  const doc = new jsPDF();
  const nameValue = name.value;
  const surnameValue = surname.value;
  const phoneNumberValue = phoneNumber.value;

  doc.text('ข้อมูลส่วนตัว', 10, 10);
  doc.text(`ชื่อ: ${nameValue}`, 10, 20);
  doc.text(`นามสกุล: ${surnameValue}`, 10, 30);
  doc.text(`เบอร์โทร: ${phoneNumberValue}`, 10, 40);

  doc.save('ข้อมูลส่วนตัว.pdf');
};
</script>
