<template>
  <!-- Main Container for File Upload -->
  <div class="uploader-container">
    <!-- Label acts as the visual representation of the file input. Clicking this label triggers the input field -->
    <label class="file-container">
      <!-- Conditional display: Show prompt text if no file is chosen, else show the name of the chosen file -->
      <span v-if="!fileName">Click to select an Excel file</span>
      <span v-else>{{ fileName }}</span>
      <!-- Actual File Input: Remains hidden but functional -->
      <input
        ref="fileInput"
        type="file"
        @change="handleFile"
        class="file-input"
      />
    </label>
  </div>
</template>

<script>
// Required imports
import { ref } from "vue";
import * as XLSX from "xlsx";

export default {
  name: "ExcelImporter",
  emits: ["update-stores"], // Event emitter for updating stores after reading Excel
  setup(_, { emit }) {
    // Refs for file input element and the name of the selected file
    const fileInput = ref(null);
    const fileName = ref(null);

    // Method to handle when a file is selected
    const handleFile = () => {
      // Retrieve the selected file
      const file = fileInput.value.files[0];
      // Update the fileName ref with the chosen file's name for display purposes
      fileName.value = file.name;

      // Create a FileReader instance to read the contents of the selected file
      const reader = new FileReader();

      // Event listener for when the file read operation completes
      reader.onload = (e) => {
        const data = new Uint8Array(e.target.result);
        // Parse the read data using XLSX to get workbook
        const workbook = XLSX.read(data, { type: "array" });

        // Take the first worksheet from the workbook
        const worksheetName = workbook.SheetNames[0];
        const worksheet = workbook.Sheets[worksheetName];

        // Convert the worksheet data into JSON format
        const json = XLSX.utils.sheet_to_json(worksheet);

        // Emit an event to update the stores with the parsed data
        emit("update-stores", json);
      };

      // Initiate reading the file as an ArrayBuffer
      reader.readAsArrayBuffer(file);
    };

    // Expose the necessary refs and methods to the template
    return {
      fileInput,
      handleFile,
      fileName,
    };
  },
};
</script>

<style scoped>
/* Styles for the main file upload container */
.uploader-container {
  margin: 20px 0;
  text-align: center;
}

/* Styles for the visual representation of the file input */
.file-container {
  position: relative;
  background: #fff;
  padding: 10px 20px;
  border-radius: 6px;
  border: 1px solid #eaeaea;
  display: inline-block;
  cursor: pointer;
  transition: background-color 0.3s;
}

.file-container:hover {
  background-color: #f1f1f1;
}

/* Styles for the actual (hidden) file input. It's overlayed on top of the label with opacity set to 0. */
.file-input {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  cursor: pointer;
}
</style>
