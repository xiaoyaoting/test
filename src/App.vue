<template>
  <div id="app">
    <Test msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import Test from './components/Test.vue';

export default {
  name: 'App',
  components: {
    Test,
  },
  data() {
    return {
      blocklet: window?.blocklet,
    };
  },
};
</script>

<style>
html,body, #app {
  width: 100vw;
  height: 100vh;
}
</style>
