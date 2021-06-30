<template>
  <div id="app">
    <IndexView />
  </div>
</template>

<script>
import IndexView from './views/IndexView.vue';

export default {
  name: 'App',
  components: {
    IndexView,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
