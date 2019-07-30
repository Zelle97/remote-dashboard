<template>
  <div id="app">
    <div class="container-fluid">
      <div class="main-view">

        <div class="row">
          <div class="col-4"></div>
          <div class="col-4">
            <h1>Remote Dashboard</h1>
          </div>
          <div class="col-4"></div>
        </div>

        <div class="row host-view">
          <table class="table">
            <thead>
            <tr>
              <th scope="col">Host</th>
              <th scope="col">Status</th>
              <th scope="col">Actions</th>
            </tr>
            </thead>
            <tbody>

            <tr v-for="entry in items">
              <td>{{entry.name}}</td>
              <td v-bind:class="{ 'status-online': entry.online,'status-offline': !entry.online }" class="status">
                {{getText(entry)}}
              </td>
              <td>
                <button class="btn btn-success" type="button" v-on:click="startHost(entry)">Start</button>
                <button class="btn btn-danger" type="button" v-on:click="shutdownHost(entry)">Shutdown</button>
              </td>
            </tr>

            </tbody>
          </table>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  const backendUrl = "http://localhost:8888";

  export default {
    name: 'app',
    data() {
      return {
        items: []
      }
    },
    beforeMount() {
      this.getHosts();
    },
    methods: {
      getHosts() {
        let url = backendUrl + "/hosts";
        axios({method: "get", "url": url}).then(result => {
          this.items = result.data;
        }, error => {
          console.error(error);
        });
      },
      startHost(entry) {
        let url = backendUrl + "/wake/" + entry.name;
        axios({method: "POST", "url": url}).then(result => {
          console.log(result.data.origin);
        }, error => {
          console.error(error);
        });
      },
      shutdownHost(entry) {
        let url = backendUrl + "/shutdown/" + entry.name;
        axios({method: "POST", "url": url}).then(result => {
          console.log(result.data.origin);
        }, error => {
          console.error(error);
        });
      },
      getText(entry) {
        this.checkHost(entry);
        for (let i = 0; i < this.items.length; i++) {
          if (this.items[i].name == entry.name) {
            if (this.items[i].online) {
              return 'Online'
            } else {
              return 'Offline'
            }
          } else {
            console.log('Host ' + name + ' not found')
          }
        }
      },
      checkHost(entry) {
        let url = backendUrl + "/status/" + entry.name;
        axios({method: "GET", "url": url}).then(result => {

          let name = result.data.name;
          let status = result.data.status;

          for (let i = 0; i < this.items.length; i++) {
            if (this.items[i].name == name) {
              if (status == 'Online') {
                this.items[i].online = true;
              } else {
                this.items[i].online = false;
              }
            } else {
              console.log('Host ' + name + ' not found')
            }
          }
        }, error => {
          console.error(error);
        });
      }
    }
  }
</script>

<style>
  .table {
    text-align: center;
  }

  .status-offline {
    color: red;
  }

  .status-online {
    color: green;
  }

  .main-view {
    margin: 50px;
  }

  .host-view {
    padding: 25px;
  }

</style>
