# simple-api-table

Simple Vue 2 table to fetch and sort data from REST API.

# Dependencies

* [axios](https://github.com/mzabriskie/axios)
* [lodash](https://github.com/lodash/lodash)

# Demo

![Demo](http://i.imgur.com/KStnrCV.gif)

# Installation

```bash
npm install simple-api-table
```

then, in your `main.js`:

```javascript
import SimpleApiTable from 'simple-api-table'
Vue.component('simple-api-table', SimpleApiTable)
```

# Usage

```
<simple-api-table url="/your/url/here"> </simple-api-table>
```

# API

| Option | Value | Default | Description |
| ------ | ------ | ------ | ------ |
| `url` | `http://your/url` or `/your/url` | `undefined` (required) | Specifies the API Url
| `ordered` | `true` or `false`| `true` | Display records with order numbers
| `header-case` | `start`, `camel`, `kebab` | `start` | Transform keys to a case for table headings
| `sort-by-default` | The key you want to sort by | First key | Key the table will be sorted by on render
| `sort-dir-default` | `asc` or `desc` | `asc` | Direction the table will be sorted in on render
| `except-keys` | Array of keys | `[]` | Keys you dont want to be displayed

# License

[MIT](https://opensource.org/licenses/MIT)
