<template>
    <div>
     
        <button @click="downloadPDF(index)">Download PDF</button>
    </div>
</template>

<script setup>
// import { ref } from 'vue';
import { jsPDF } from "jspdf";
import { font } from "./THSarabun-normal";

let index = "AAAAAAAAAAAAAAAA"

const downloadPDF = (index) => {
    const doc = new jsPDF();

    // Add custom font
    doc.addFileToVFS("MyFont.ttf", font);
    doc.addFont("MyFont.ttf", "MyFont", "normal");
    doc.setFont("MyFont");

    // Text strings
    const textTopRight = "INTERNTAL USE";
    const textTopCenter = "TAKAHATAPRECISION(THAILAND)LTD.";
    const textTop = "ใบแจ้งระบบแจ้งซ่อม";
    const textBottomLeft = "FM-EDP-17 REV :04";
    const employeeNameTitle = "รายชื่อพนังงานเจ้าหน้าที่:";
    const itManagerNameTitle = "รายชื่อหัวหน้างานแผนกไอที:";

    // Page dimensions
    const pageWidth = doc.internal.pageSize.getWidth();
    const pageHeight = doc.internal.pageSize.getHeight();

    // Positions
    const margin = 10;
    const topMargin = 10;
    const spacing = 5;

    // Position the text "INTERNTAL USE" at the top-right corner
    doc.setFontSize(15); // Set font size for the top-right text
    const textTopRightWidth = doc.getTextWidth(textTopRight);
    doc.text(textTopRight, pageWidth - textTopRightWidth - margin, topMargin);

    // Position "TAKAHATAPRECISION(THAILAND)LTD." at the center below "INTERNTAL USE"
    doc.setFontSize(12); // Set font size for the top-center text
    const textTopCenterWidth = doc.getTextWidth(textTopCenter);
    doc.text(textTopCenter, (pageWidth - textTopCenterWidth) / 2, topMargin + spacing + 10);

    doc.setFontSize(14); // Set font size for the top text
    const textTopWidth = doc.getTextWidth(textTop);
    doc.text(textTop, (pageWidth - textTopWidth) / 2, topMargin + spacing + 19);

    // Position the text "Hello world! คุญัญญา" somewhere on the page
    doc.setFontSize(12); // Set font size for the middle text
    doc.text("Hello world! คุญัญญา", margin, topMargin + spacing + 30); // Adjusted y-position to avoid overlap
    doc.text(index, 10, 15)
    // Position the text "รายชื่อพนังงานเจ้าหน้าที่:" and "รายชื่อหัวหน้างานแผนกไอที:" just above "FM-EDP-17 REV :04"
    const bottomTextY = pageHeight - margin;
    doc.setFontSize(12); // Set font size for the bottom-left texts
    doc.text(employeeNameTitle, margin, bottomTextY - 30); // Adjust the Y position for spacing
    doc.text(itManagerNameTitle, margin, bottomTextY - 20); // Adjust the Y position for spacing

    // Position the text "FM-EDP-17 REV :04" at the bottom-left corner
    doc.setFontSize(15); // Set font size for the bottom-left text
    doc.text(textBottomLeft, margin, bottomTextY);

    doc.save("test.pdf");
};
</script>
