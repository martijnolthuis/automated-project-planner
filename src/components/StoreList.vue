<template>
  <div>
    <h2>Store List</h2>
    <ExcelImporter @update-stores="updateStores" />

    <!-- All Stores Table -->
    <table>
      <thead>
        <tr>
          <th>All Stores</th>
          <th>Number of Employees</th>
        </tr>
      </thead>
      <tbody>
        <draggable
          :list="stores"
          group="stores"
          :move="checkMove"
          class="drag-area"
        >
          <template #item="{ element, index }">
            <tr :key="index">
              <td>{{ element.storename }}</td>
              <td>{{ element.employees }}</td>
            </tr>
          </template>
        </draggable>
        <tr v-if="!stores.length">
          <td colspan="2">No stores imported yet.</td>
        </tr>
      </tbody>
    </table>

    <!-- Days and their stores -->
    <div v-for="(dayStores, day) in weekdays" :key="day">
      <h3>
        {{ day }} - Total Employees:
        {{ getTotalEmployees(dayStores) }}
      </h3>
      <table>
        <thead>
          <tr>
            <th>Store Name</th>
            <th>Number of Employees</th>
          </tr>
        </thead>
        <tbody>
          <draggable
            :list="dayStores"
            group="stores"
            :move="checkMove"
            class="drag-area"
          >
            <template #item="{ element, index }">
              <tr :key="index">
                <td>{{ element.storename }}</td>
                <td>{{ element.employees }}</td>
              </tr>
            </template>
          </draggable>
          <tr v-if="!dayStores.length">
            <td colspan="2">No stores assigned yet</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// import { ref } from "vue";
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
      stores: [],
      weekdays: {
        Monday: [],
        Tuesday: [],
        Wednesday: [],
        Thursday: [],
        Friday: [],
        Saturday: [],
        Sunday: [],
      },
    };
  },
  methods: {
    checkMove(evt) {
      return evt.draggedContext.index != evt.relatedContext.index;
    },
    getTotalEmployees(dayStores) {
      return dayStores.reduce((total, store) => {
        if (store && store.employees) {
          return total + store.employees;
        }
        return total;
      }, 0);
    },
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
.drag-area {
  width: 100%;
}

.store-row {
  transition: background-color 0.3s;
  cursor: pointer;
}

.store-row:hover {
  background-color: #e9ecef;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

th,
td {
  padding: 10px;
  border: 1px solid #dee2e6;
  text-align: left;
}

thead {
  background-color: #f8f9fa;
}

h2,
h3 {
  text-align: center;
  margin-bottom: 20px;
}
</style>

<style scoped>
.drag-area {
  width: 100%;
}

.store-row {
  transition: background-color 0.3s;
  cursor: pointer;
}

.store-row:hover {
  background-color: #e9ecef;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

th,
td {
  padding: 10px;
  border: 1px solid #dee2e6;
  text-align: left;
}

thead {
  background-color: #f8f9fa;
}

h2,
h3 {
  text-align: center;
  margin-bottom: 20px;
}
</style>

<style scoped>
.drag-area {
  width: 100%;
}
.store-row {
  transition: background-color 0.3s;
  cursor: pointer;
}

.store-row:hover {
  background-color: #e9ecef;
}

table {
  border-collapse: collapse;
  width: 60%;
  margin: 0 auto;
  table-layout: fixed;
}

th,
td {
  padding: 12px;
  text-align: left;
  border: 1px solid #ddd;
}

th {
  background-color: #f2f2f2;
  white-space: nowrap; /* add this line to prevent text wrapping */
}

td {
  width: 200%; /* add this line to make the td elements as wide as the th elements */
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #ddd;
}

thead {
  background-color: #f8f9fa;
}

h2,
h3 {
  text-align: center;
  margin-bottom: 20px;
}
</style>
