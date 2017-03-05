# simple-api-table

Simple Vue table to fetch and sort data from REST API.

# Dependencies

* [axios](https://github.com/mzabriskie/axios)
* [lodash](https://github.com/lodash/lodash)

# Installation

To be added

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

# License

[MIT](https://opensource.org/licenses/MIT)
