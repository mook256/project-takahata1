<template>
    <div>
      <h1>k</h1>
      <button @click="downloadPDF">Download PDF</button>
    </div>
  </template>
  
  <script setup>

  import { jsPDF } from "jspdf";
  import { font } from "./Sarabun_1";
  
  const downloadPDF = async () => {
      try {
          const response = await fetch('http://172.29.10.238:8080/api/IT_Repair_data_table', {
              method: 'POST',
              credentials: 'include',
          });
          const data = await response.json();
  
          // กรองข้อมูลเพื่อเลือกเฉพาะบุคคลที่ต้องการ
          const userId = '123'; // กำหนด user_id ของบุคคลที่ต้องการ
          const userData = data.results.find(item => item.user_id === userId);
  
          if (!userData) {
              console.error('User not found');
              return;
          }
  
          const doc = new jsPDF();
  
          // Add custom font
          doc.addFileToVFS("MyFont.ttf", font);
          doc.addFont("MyFont.ttf", "MyFont", "normal");
          doc.setFont("MyFont");
  
          // Text strings
          const textTopRight = "INTERNAL USE";
          const textTopCenter = "TAKAHATA PRECISION (THAILAND) LTD.";
          const textTop = "ใบแจ้งระบบแจ้งซ่อม";
          const textBottomLeft = "FM-EDP-17 REV :04";
  
          // Page dimensions
          const pageWidth = doc.internal.pageSize.getWidth();
          const pageHeight = doc.internal.pageSize.getHeight();
  
          // Text widths
          const textTopRightWidth = doc.getTextWidth(textTopRight);
          const textTopCenterWidth = doc.getTextWidth(textTopCenter);
          const textTopWidth = doc.getTextWidth(textTop);
          
          // Positions
          const margin = 10;
          const topMargin = 10;
          const spacing = 5;
  
          // Position the text "INTERNTAL USE" at the top-right corner
          doc.text(textTopRight, pageWidth - textTopRightWidth - margin, topMargin);
  
          // Position "TAKAHATA PRECISION (THAILAND) LTD." at the center below "INTERNTAL USE"
          doc.text(textTopCenter, (pageWidth - textTopCenterWidth) / 2, topMargin + spacing + 10);
          doc.text(textTop, (pageWidth - textTopWidth) / 2, topMargin + spacing + 19);
  
          // Insert fetched data for the specific user
          const startingY = topMargin + spacing + 30;
          const lineSpacing = 10;
  
          doc.text(`Job No: ${userData.job_no}`, margin, startingY);
          doc.text(`EN: ${userData.en}`, margin + 50, startingY);
          doc.text(`Requester: ${userData.requester}`, margin + 100, startingY);
          doc.text(`Section: ${userData.section}`, margin + 150, startingY);
          // Add more fields as needed
  
          // Position the text "FM-EDP-17 REV :04" at the bottom-left corner
          doc.text(textBottomLeft, margin, pageHeight - margin);
  
          doc.save("test.pdf");
      } catch (error) {
          console.error('Failed to fetch data:', error);
      }
  };
  </script>
  