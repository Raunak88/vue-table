<template>
  <section class="vue-table">
    <table>
      <thead>
        <tr>
          <th v-for="(obj, ind) in config" :key="ind">{{ obj.title }}</th>
        </tr>
      </thead>
      <tbody v-if="childData.length>1">
        <tr v-for="(row, index) in childData" :key="index">
          <td v-for="(obj, ind) in config" :key="ind">
            <span v-if="!obj.type">{{row[obj.key]}}</span>
            <span v-if="obj.type === 'date'">{{new Date(row[obj.key]).toDateString()}}</span>
          </td>
        </tr>
      </tbody>
      <tbody v-if="!childData.length">
        <tr>            
          <td>No Data Found</td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
export default {
  props: ["childData", "config"],
};
</script>

<style >
.vue-table {
  border: 1px solid #999;
  color: #333;
  overflow: auto;
  margin-top: 30px;
}
.vue-table table {
  border-collapse: collapse;
  width: 100.1%;
}
.vue-table table th {
  position: sticky;
  top: 0;
  background: #eda806;
  padding: 10px 5px;
  text-align: left;
  border-bottom: 1px solid #999;
  padding: 0.75rem;
  vertical-align: top;
}

.vue-table table td {
  padding: 5px 5px;
  text-align: left;
  padding: 0.75rem;
  vertical-align: top;
}
</style>