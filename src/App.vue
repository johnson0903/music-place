<template>
  <div id="app">
    <Home msg="我的小空間" />
  </div>
</template>

<script>
import Home from './pages/Home';
export default {
  name: 'App',
  components: {
    Home,
  },
};
</script>

<style>
body,
ul {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: rgb(44, 62, 80);
}
</style>
