<template>
  <div id="app">
   <Table v-if="tableData" :childData="tableData" :configData="config"/>
  </div>
</template>

<script>
import Table from './components/Table';
export default {
  name: 'App',
  components: {
    Table
  },  
  data:()=>({
    tableData:undefined,
    config:[
      {
        key:'name',
        title:'Name'
      },
       {
        key:'email',
        title:'Email'
      },
       {
        key:'phone',
        title:'Phone'
      },
       {
        key:'company',
        title:'Company'
      },

       {
        key:'createdAt',
        title:'Creation Date',
        type:'date'
      }
    ]  

    
  }),
  mounted(){
    this.$axios.get('https://5f40a2dea5e9db0016301f4a.mockapi.io/assignment/users')
    .then((res)=>{
      this.tableData=res.data;
    })
    .catch((err)=>console.log(err))
  }
}
</script>

<style>

</style>
