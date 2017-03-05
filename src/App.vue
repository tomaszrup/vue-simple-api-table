<template>
  <div id="app">
    <simple-api-table sort-by-default="title" url="https://jsonplaceholder.typicode.com/posts"></simple-api-table>
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
