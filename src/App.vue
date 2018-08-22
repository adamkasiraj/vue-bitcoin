<template>
  <div id="app">
    <BitcoinTracker/>
  </div>
</template>

<script>
import BitcoinTracker from '@/components/BitcoinTracker'
import BitcoinCard from '@/components/BitcoinCard'

export default {
  name: 'App',
  components: {
    BitcoinTracker,
    BitcoinCard
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: #F1F6F8;
}
</style>
