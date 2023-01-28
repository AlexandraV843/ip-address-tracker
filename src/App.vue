<template>
  <IPinfo/>
</template>

<script>
import IPinfo from './components/IPinfo.vue'


export default {
  name: 'App',
  components: {
    IPinfo,
  }
}
</script>

<style>
  @import '~bootstrap/dist/css/bootstrap.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
