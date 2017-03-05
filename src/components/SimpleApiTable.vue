<template>
  <div class="simple-api-table">
    <table v-if="!loading">
      <thead>
        <tr>
          <th v-if="ordered">Num</th>
          <th v-for="key in keys" @click="sort(key)">{{humanifyHeader(key)}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="record, index in records">
          <th v-if="ordered">{{index + 1}}</th>
          <th v-for="key in keys">{{record[key]}}</th>
        </tr>
      </tbody>
    </table>
    <h1 v-if="loading">Loading...</h1>
  </div>
</template>

<script>
import _ from 'lodash'
import axios from 'axios'

export default {
  name: 'simple-api-table',
  created () {
    this.sendGetRequest()
  },
  props: {
    url: {
      type: String,
      required: true
    },
    ordered: {
      type: Boolean,
      default: true
    },
    headerCase: {
      type: String,
      default: 'start'
    },
    sortByDefault: {
      type: String
    },
    sortDirDefault: {
      type: String
    },
    filterByDefault: {
      type: String
    }
  },
  data () {
    return {
      keys: [],
      records: [],
      loading: false,
      sortDir: '',
      sortBy: ''
    }
  },
  methods: {
    sort (key = null) {
      if (key) {
        if (this.sortBy === key) this.sortDir = this.sortDir === 'asc' ? 'desc' : 'asc'
        else {
          this.sortBy = key
          this.sortDir = 'asc'
        }
      }

      this.records = _.orderBy(this.records, [object => {
        return object[this.sortBy]
      }], this.sortDir)
    },
    sendGetRequest () {
      this.loading = true
      this.records = []

      axios.get(this.url).then(response => {
        if (!response.data[0]) return console.error('Api returned nothing.')

        this.keys = _.keys(response.data[0])
        this.records = response.data

        this.sortBy = this.sortByDefault ? this.sortByDefault : this.keys[0]
        this.sortDir = this.sortDirDefault ? this.sortDirDefault : 'asc'

        this.sort()
        this.loading = false
      }).catch(error => {
        console.error(error)
      })
    },
    humanifyHeader (key) {
      return _[this.headerCase + 'Case'](key)
    }
  }
}
</script>

<style scoped>

</style>
