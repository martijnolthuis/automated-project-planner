<template>
  <!-- Root App Container -->
  <div id="app">
    <!-- Main Application Title -->
    <h1>Stocktaking Planning</h1>
    <!-- Store List Component Inclusion -->
    <StoreList />
  </div>
</template>

<script>
// Importing the StoreList component for use within this component.
import StoreList from "./components/StoreList.vue";

export default {
  name: "App", // Name of this component
  components: {
    StoreList, // Registering StoreList for use in the template
  },
};
</script>

<style>
/* Styles for the main app container */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif; /* Setting the preferred fonts */
  -webkit-font-smoothing: antialiased; /* Improving font rendering on Webkit browsers */
  -moz-osx-font-smoothing: grayscale; /* Improving font rendering on Firefox */
  text-align: center; /* Center-aligning text */
  color: #2c3e50; /* Default text color */
  margin-top: 60px; /* Top margin for spacing */
  background-color: #f7f8fa; /* A soft, muted background color */
  padding: 20px; /* Padding inside the container */
  border-radius: 12px; /* Rounded corners */
  max-width: 800px; /* Limiting maximum width */
  margin: 100px auto; /* Centering the app container with auto margins */
}

/* Styles specific to the main application title */
h1 {
  font-size: 2.5em; /* Font size for the title */
  color: #34495e; /* Color of the title text */
  font-weight: 600; /* Making the font bold */
  margin-bottom: 30px; /* Spacing below the title */
  border-bottom: 2px solid #eaecee; /* Underline below the title */
  padding-bottom: 10px; /* Padding below the title text */
}
</style>
