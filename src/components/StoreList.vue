<template>
  <!-- Main Container for Store List and Upload Functionality -->
  <div>
    <!-- Title for the Excel Importer Section -->
    <h2>Upload Store Data</h2>
    <!-- Excel Importer Component -->
    <ExcelImporter @update-stores="updateStores" />

    <!-- Display All Stores List - This section displays all the stores available from the uploaded Excel -->
    <h3>All Stores</h3>
    <div class="draggable-container">
      <draggable
        :list="stores"
        group="stores"
        :move="checkMove"
        class="drag-area"
      >
        <!-- Template for rendering each store -->
        <template #item="{ element, index }">
          <div :key="index" class="draggable-item">
            {{ element.storename }} - {{ element.employees }} Employees
          </div>
        </template>
      </draggable>
    </div>

    <!-- Draggable Lists for each Weekday -->
    <div v-for="(dayStores, day) in weekdays" :key="day">
      <!-- Display the day name and total employees for the day -->
      <h3>
        {{ day }} - Total Employees:
        {{ getTotalEmployees(dayStores) }}
      </h3>
      <div class="draggable-container">
        <draggable
          :list="dayStores"
          group="stores"
          :move="checkMove"
          class="drag-area"
        >
          <!-- Template for rendering each store under a specific weekday -->
          <template #item="{ element, index }">
            <div :key="index" class="draggable-item">
              {{ element.storename }} - {{ element.employees }} Employees
            </div>
          </template>
        </draggable>
      </div>
    </div>

    <!-- Placeholder for displaying the data in a table format without drag-n-drop functionality -->
  </div>
</template>

<script>
// Required imports for the component functionality
import draggable from "vuedraggable";
import ExcelImporter from "./ExcelImporter.vue";

export default {
  name: "StoreList",
  components: {
    draggable,
    ExcelImporter,
  },
  data() {
    return {
      stores: [], // Array to store all the stores from the uploaded Excel
      weekdays: {
        // Object for storing stores categorized by weekdays
        Monday: [],
        Tuesday: [],
        Wednesday: [],
        Thursday: [],
        Friday: [],
      },
    };
  },
  methods: {
    // Method to handle draggable constraints
    checkMove(evt) {
      return evt.draggedContext.index != evt.relatedContext.index;
    },
    // Compute the total number of employees for a given day's store list
    getTotalEmployees(dayStores) {
      return dayStores.reduce((total, store) => {
        if (store && store.employees) {
          return total + store.employees;
        }
        return total;
      }, 0);
    },
    // Update the main store list upon successful Excel import
    updateStores(newStores) {
      if (
        newStores &&
        Array.isArray(newStores) &&
        newStores.length > 0 &&
        newStores[0].storename &&
        typeof newStores[0].employees !== "undefined"
      ) {
        this.stores = newStores;
      } else {
        console.error("Incorrectly formatted stores data:", newStores);
      }
    },
  },
};
</script>

<style scoped>
.draggable-container {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 20px;
  min-height: 50px;
  max-width: 95%;
  margin: 0 auto;
  background-color: #eaecee; /* A slightly darker shade for contrast */
}

.draggable-item {
  padding: 10px 15px;
  margin: 10px auto;
  background-color: #ffffff;
  border: 1px solid #eaeaea;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.draggable-item:hover {
  background-color: #f1f1f1;
}

h2,
h3 {
  color: #333333;
  margin-top: 40px;
  text-align: center;
}

h2 {
  font-size: 2em;
}

h3 {
  font-size: 1.5em;
}

/* You can add a touch of color for accent, such as blue for actionable buttons or highlights. */
</style>

<style scoped>
/* Styles for the drag-and-drop container */
.draggable-container {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 20px;
  min-height: 50px;
  max-width: 95%;
  margin: 0 auto;
  background-color: #eaecee; /* A slightly darker shade for contrast */
}

/* Styles for individual draggable items */
.draggable-item {
  padding: 10px 15px;
  margin: 10px auto;
  background-color: #ffffff;
  border: 1px solid #eaeaea;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.draggable-item:hover {
  background-color: #f1f1f1;
}

/* Styling for the main and sub-headers */
h2,
h3 {
  color: #333333;
  margin-top: 40px;
  text-align: center;
}

h2 {
  font-size: 2em;
}

h3 {
  font-size: 1.5em;
}
</style>
