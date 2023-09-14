<template>
  <div>
    <label>
      Upload Excel:
      <input ref="fileInput" type="file" @change="handleFile" />
    </label>
  </div>
</template>
<script>
import { ref } from "vue";
import * as XLSX from "xlsx";

export default {
  name: "ExcelImporter",
  emits: ["update-stores"],
  setup(_, { emit }) {
    const fileInput = ref(null);

    const handleFile = () => {
      const file = fileInput.value.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        const data = new Uint8Array(e.target.result);
        const workbook = XLSX.read(data, { type: "array" });

        const worksheetName = workbook.SheetNames[0];
        const worksheet = workbook.Sheets[worksheetName];

        const json = XLSX.utils.sheet_to_json(worksheet);

        emit("update-stores", json);
      };

      reader.readAsArrayBuffer(file);
    };

    return {
      fileInput,
      handleFile,
    };
  },
};
</script>
