<template>
  <div class="simple-api-table">
    <div class="show-select">
      Show
        <select v-model="perPage" @change="currentPage = 1">
          <option value="10">10</option>
          <option value="50">50</option>
          <option value="100">100</option>
          <option value="500">500</option>
        </select>
    </div>
    <table v-if="!loading">
      <thead>
        <tr>
          <th v-if="ordered">{{humanifyHeader('num')}}</th>
          <th v-for="key in keys" @click="sort(key)">
            {{humanifyHeader(key)}}
            <span v-if="sortBy === key" class="caret">{{caret}}</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="record, index in records" v-if="isOnCurrentPage(index)">
          <td v-if="ordered">{{index + 1}}</td>
          <td v-for="key in keys">{{record[key]}}</td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button v-for="page in totalPages" :class="{'active': currentPage === page}" @click="currentPage = page">{{page}}</button>
    </div>
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
      type: String,
      default: 'asc'
    },
    exceptKeys: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      keys: [],
      records: [],
      loading: false,
      sortDir: '',
      sortBy: '',
      perPage: 50,
      currentPage: 1
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

        this.keys = this.removeSpecifiedKeys(_.keys(response.data[0]))
        this.records = response.data

        this.sortBy = this.sortByDefault ? this.sortByDefault : this.keys[0]
        this.sortDir = this.sortDirDefault

        this.sort()
        this.loading = false
      }).catch(error => {
        console.error(error)
      })
    },
    removeSpecifiedKeys (keys) {
      for (var i = keys.length; i >= 0; i--) {
        if (this.exceptKeys.includes(keys[i])) keys.splice(i, 1)
      }
      return keys
    },
    humanifyHeader (key) {
      return _[this.headerCase + 'Case'](key)
    },
    isOnCurrentPage (index) {
      return index >= (this.currentPage - 1) * this.perPage && index < this.currentPage * this.perPage
    }
  },
  computed: {
    caret () {
      if (this.sortDir === 'asc') return '▲'
      return '▼'
    },
    totalPages () {
      return Math.ceil(this.records.length / this.perPage)
    }
  }
}
</script>

<style scoped>
  .simple-api-table {
    font-size: 0.9em;
    margin: 0 auto;
    color: #333;
  }

  .simple-api-table table {
    text-align: left;
    background-color: #fff;
    padding: 5px;
    border-spacing: 0px;
  }

  .simple-api-table thead {
    padding: 5px;
    font-weight: bold;
    color: #888;
    font-family: 'Helvetica';
     user-select: none;
  }

  .simple-api-table thead th {
    white-space: nowrap;
    padding: 10px;
    border-bottom: 1px solid #555;
    cursor: pointer;
  }

  .simple-api-table thead th:first-child {
    cursor: default;
  }

  .simple-api-table tbody tr:nth-child(even){
    background-color: #eee;
  }

  .simple-api-table td {
    padding: 10px;
    font-family: Arial
  }

  .pagination .active {
    background-color: orangered;
  }

</style>
