<template>
  <div id="app">
    <top-header title="Рассчет валют"></top-header>
    <main-screen></main-screen>
    <main-footer></main-footer>
  </div>
</template>

<script>
import MainScreen from './pages/MainScreen.vue';
import TopHeader from './pages/Layout/TopHeader.vue';
import MainFooter from './pages/Layout/MainFooter.vue';

export default {
  name: 'App',
  components: {
    MainScreen,
    TopHeader,
    MainFooter,
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
