<template>
<v-app>
  <v-data-table
    :headers="headers"
    :items="customers"
    :items-per-page="5"
    class="elevation-1"
    @click:row="handleClickProject"
  ></v-data-table>

</v-app>
</template>

<script>
import axios from 'axios';
import store from '../store';
export default {
    methods : {
        handleClickProject : function (row) {
            this.selectedCustomer = true
            store.state.customerId = row.customerId
            this.$router.push('/customerProjects')
        },
    },
 async mounted () {
     await axios
     .get('http://localhost:8080/customer/customerList')
     .then(response => {
         response.data.forEach(element => {
             this.customers.push({
                 name : element.customerName,
                 projects : element.projects.length,
                 customerId : element.customerId
             })
         });
     })
     this.customers = this.customers.slice(1,)
 },
 data () {
      return {
          selectedCustomer: null,
        headers: [
          {
            text: 'Customer',
            align: 'start',
            sortable: true,
            value: 'name',
          },
          { text: 'Projects', value: 'projects' },
          
        ],
        
        customers: [{

        }],
      }
    },
    
}

</script>

<style>

</style>