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

    <!-- Button to trigger store assignment across weekdays -->
    <!-- It will only render if there are no stores in any weekdays -->
    <button
      @click="assignStoresToDays"
      class="assign-button"
      v-if="!hasStoresInWeekdays"
    >
      Assign Stores to Days
    </button>
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
  computed: {
    /**
     * Checks if any weekday has stores assigned.
     * Returns true if at least one weekday has a store.
     */
    hasStoresInWeekdays() {
      return Object.values(this.weekdays).some(
        (dayStores) => dayStores.length > 0,
      );
    },
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

    /**
     * Assigns stores to weekdays.
     * This method handles the logic of distributing the stores evenly across the week based on the
     * employee count to ensure a balanced workload throughout the week.
     */
    assignStoresToDays() {
      // 1. Sort stores in descending order by employees
      const sortedStores = [...this.stores].sort(
        (a, b) => b.employees - a.employees,
      );

      // 2. Create a helper array for the weekdays
      const dayBins = [
        { day: "Monday", value: 0, stores: [] },
        { day: "Tuesday", value: 0, stores: [] },
        { day: "Wednesday", value: 0, stores: [] },
        { day: "Thursday", value: 0, stores: [] },
        { day: "Friday", value: 0, stores: [] },
      ];

      // 3. Assign each store to the day with the smallest total employees
      for (const store of sortedStores) {
        dayBins.sort((a, b) => a.value - b.value);
        dayBins[0].stores.push(store);
        dayBins[0].value += store.employees;
      }

      // 4. Update the main weekdays object with the result
      for (const bin of dayBins) {
        this.weekdays[bin.day] = bin.stores;
      }

      // Optional: Clear the all stores list after assignment
      this.stores = [];
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

/* Button to assign stores to weekdays */
.assign-button {
  padding: 10px 20px;
  font-size: 1.2em;
  color: #fff;
  background-color: #3498db; /* Base color for the button */
  border: none;
  border-radius: 6px;
  margin-top: 20px;
  cursor: pointer;
  transition: background-color 0.3s; /* Transition effect for smooth color change */
  outline: none; /* Remove the default focus outline for aesthetics */
}

/* Hover effect for the assign button */
.assign-button:hover {
  background-color: #2980b9; /* Darkened color for hover effect */
}

/* Active state effect for the assign button */
.assign-button:active {
  background-color: #2471a3; /* Further darkened color for active (pressed) state */
}
</style>
