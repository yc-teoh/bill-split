<template>
  <BillSplit msg="Bill Split"/>
</template>

<script>
import BillSplit from './components/BillSplit.vue'

export default {
  name: 'App',
  components: {
    BillSplit
  }
}
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
