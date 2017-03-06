<template>
  <div id="app">
    <simple-api-table url="https://jsonplaceholder.typicode.com/posts"></simple-api-table>
  </div>
</template>

<script>
import SimpleApiTable from './components/SimpleApiTable'

export default {
  name: 'app',
  components: {
    SimpleApiTable
  }
}
</script>
<style media="screen">
  #app {
    max-width: 80%;
    margin: 0 auto;
  }
</style>
