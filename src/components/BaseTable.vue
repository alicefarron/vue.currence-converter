<template>
  <table class="table tablesorter">
    <thead>
      <tr>
        <slot name="columns">
          <th v-for="column in columns" :key="column.name"> {{ column.name }}</th>
        </slot>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in data" :key="index">
        <slot :row="item">
          <td v-for="(column, index) in columns"
              :key="index"
              >
            {{itemValue(item, column.value)}}
          </td>
        </slot>
      </tr>
    </tbody>
  </table>
</template>
<script>
 export default {
   name: 'base-table',
   props:{
     data:{
       type: Array,
       default: () => []
     },
     columns:{
       type: Array,
       default: () => []
     }
   },
   methods: {
     hasValue(item, column) {
       return item[column.toLowerCase()] !== "undefined";
     },
     itemValue(item, column) {
       return item[column.toLowerCase()];
     }
   }
 }
</script>
<style>
.table {
    width: 100%;
}

thead {
    border: 1px solid #818181;
}

td {
    border: 1px solid #818181;
    padding: 10px;
}
</style>
