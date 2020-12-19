<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <vue-good-table ref="salut"
      :columns="columns"
      :rows="rows"
      max-height="600px"
      :fixed-header="true"
      :row-style-class="rowStyleClassFn"
      theme="black-rhino"
      styleClass="vgt-table striped condensed"
      :search-options="{
        enabled: true
      }"
      :select-options="{
        enabled: true,
        disableSelectInfo: true
      }"
    >
      <div slot="table-actions">
        <a @click.prevent="doAction1" href="javascript:void(0)">Action1</a>
        <a @click.prevent="resetFilters" href="javascript:void(0)">Reset</a>
      </div>
      <div slot="table-actions-bottom">
        This will show up on the bottom of the table.
      </div>
      <div slot="emptystate">
        No data found
      </div>
      <template slot="table-column" slot-scope="props">
        <span v-if="props.column.label == 'Icon'">
          <i class="fa fa-address-book"></i>fontawesome {{props.column.label}}
        </span>
          <span v-else>
          {{props.column.label}}
        </span>
      </template>
    </vue-good-table>

  </div>
</template>

<script>
import 'vue-good-table/dist/vue-good-table.min.css'
import { VueGoodTable } from 'vue-good-table'
import flatpickr from 'flatpickr'

import 'flatpickr/dist/flatpickr.css'
import 'flatpickr/dist/themes/material_blue.css'

export default {
  name: 'HelloWorld',
  data () {
    return {
      columns: [
        {
          label: 'Test1',
          field: 'lalala'
        },
        {
          label: 'Test2',
          field: 'lolo',
          type: 'number',
          globalSearchDisabled: true
        },
        {
          label: 'Test3',
          field: 'dd',
          type: 'date',
          globalSearchDisabled: true,
          dateInputFormat: 'yyyy-MM-dd',
          dateOutputFormat: 'MMM do yyyy',
          filterOptions: {
            enabled: true,
            filterFn: function (data, filterString) {
              const dateRange = filterString.split(' to ')
              if (dateRange.length === 2) {
                const startDate = Date.parse(dateRange[0])
                const endDate = Date.parse(dateRange[1])
                const dataData = Date.parse(data)
                return dataData >= startDate && dataData <= endDate
              }
              return true
            }
          }
        },
        {
          label: 'Icon',
          field: 'icon',
          formatFn: function (value) {
            return value ? 'oui' : 'non'
          },
          filterOptions: {
            enabled: true,
            placeholder: 'All',
            filterDropdownItems: [
              { value: true, text: 'oui' },
              { value: false, text: 'non' }
            ]
          }
        }
      ],
      rows: [
        { id: 1, lalala: 'oui', lolo: 8, dd: '2020-12-18', icon: true },
        { id: 2, lalala: 'oui2', lolo: 88, dd: '2020-06-05', icon: true },
        { id: 3, lalala: 'oui3', lolo: 11, dd: '2020-05-06', icon: false },
        { id: 4, lalala: 'oui4', lolo: 6, dd: '2019-12-18', icon: true }
      ]
    }
  },
  props: {
    msg: String
  },
  components: {
    VueGoodTable
  },
  methods: {
    rowStyleClassFn (row) {
      return row.lolo > 50 ? 'red' : ''
    },
    handleCustomFilter (value) {
      return value
    },
    doAction1 (e) {
      console.log(e)
      const ids = this.$refs.salut.selectedRows.map((row) => {
        return row.id
      })
      alert(ids.join(','))
    },
    resetFilters (e) {
      console.log(e)
      this.$refs.salut.globalSearchTerm = ''
      this.$refs.salut.reset()
    }
  },
  mounted () {
    flatpickr('input[placeholder="Filter Test3"]', {
      dateFormat: 'Y-m-d',
      mode: 'range'
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.vgt-global-search__actions {
  color: white;
}

tr.red {
  color: blue;
}

tr {
  &.red {
    td {
      background-color: red;
    }
  }
}

</style>
