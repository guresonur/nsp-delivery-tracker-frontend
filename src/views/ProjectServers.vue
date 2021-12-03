<template>
  <v-app>
      <v-data-table     
    :headers="headersServers"
    :items="servers"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>
  

  <v-file-input
    label="JSON file"
    value="serverFileUpload"
  ></v-file-input>

  <button v-on:click="handleClick">this is a button</button>
  </v-app>
</template>

<script>

import axios from "axios";
    import store from '../store'

export default {
    mounted () {
        this.getServersByProject(store.state.projectId)
        this.servers = this.servers.slice(1,)
        console.log(this.servers)
    },
data () {
        return {
            serverFile : null,
            projectId : undefined,
            servers: [{

        }],
                    headersServers : [
            {
                text: "Server ID",
                align: 'start',
                sortable: true,
                value: 'serverId'
            },
            {
                text: 'Hostname', value: 'hostname'
            },
            {
                text: 'SELinux', value: 'selinuxStatus'
            },
            {
                text: 'RHEL version', value: 'rhelRel'
            }
        ],
        }
    },
    methods : {
        async getServersByProject(id) {
            await 
            axios
            .get('http://localhost:8080/server/serversProject/'+id)
            .then(response => {
                response.data.forEach(element => {
                    console.log(element)
                    this.servers.push({
                        serverId : element.serverId,
                        hostname : element.hostname,
                        selinuxStatus : element.selinuxStatus,
                        rhelRel : element.rhelRel
                    })
                })
            })
        },
        handleClick () {
            console.log('submit clicked!')
        }
    },
}
</script>

<style>

</style>