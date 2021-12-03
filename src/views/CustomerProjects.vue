<template>
  <v-app>
      <v-data-table     
    :headers="headersProjects"
    :items="projects"
    :items-per-page="5"
    class="elevation-1"
    @click:row="handleClickProject"
  ></v-data-table>
  </v-app>

</template>

<script>

    import axios from "axios";
    import store from '../store'

export default {
    mounted () {
        this.getProjectsByCustomer(store.state.customerId)
        this.projects = this.projects.slice(1,)
        console.log(this.projects)
    },
    data () {
        return {
            customerId : undefined,
            projects: [{

        }],
                    headersProjects : [
            {
                text: "Project ID",
                align: 'start',
                sortable: true,
                value: 'projectId'
            },
            {
                text: 'Project Description', value: 'projectDescription'
            },
            {
                text: 'Servers', value: 'servers'
            }
        ],
        }
    },
    props:
    {
        customerIdX: null
    },
    methods : {
        async getProjectsByCustomer(id) {
            await 
            axios
            .get('http://localhost:8080/project/projectsCustomer/'+id)
            .then(response => {
                response.data.forEach(element => {
                    console.log(element)
                    this.projects.push({
                        projectId : element.projectId,
                        projectDescription : element.projectDescription,
                        servers : element.servers.length
                    })
                })
            })
        },
        handleClickProject : function (row) {
           this.selectedProjectId = true
            store.state.projectId = row.projectId
            this.$router.push('/projectServers')
            
        },

    },
    
}
</script>

<style>

</style>