<template>
  <h1>Stocktaking Planning</h1>
  <StoreList />
</template>

<script>
import StoreList from './components/StoreList.vue';

export default {
  name: 'App',
  components: {
    StoreList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
