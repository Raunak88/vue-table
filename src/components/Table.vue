<template>
 <table>
   <thead>
     <tr>
       <th v-for="(col,index) in configData" :key="index">{{col.title}}</th>
     </tr>
   </thead>
   <tbody>
     <tr v-for="(person,i) in childData" :key="i">
       <td v-for="(col,index) in configData" :key="index">
         <span v-if="!col.type">{{person[col.key]}}</span>
         <span v-if="col.type==='date'">{{new Date(person[col.key]).toDateString()}}</span>
       </td>
     </tr>
   </tbody>
 </table>
</template>

<script>
export default{
  name:'Table',
  props:['childData','configData']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
