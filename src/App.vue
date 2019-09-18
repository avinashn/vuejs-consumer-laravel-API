<template>
  <div>
    <table>
      <tr>
        <th>id</th>
        <th>name</th>
        <th>email</th>
      </tr>
      <tr v-for="item in user_data">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.email}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
  import Vue from 'vue'
  import axios from 'axios'; 
  Vue.use(axios);

  export default {
    name: 'app',
    data: function() {
      return {
        access_token: '',
        user_data: ''
      }
    },
    methods:{
      get_data: function(){
        axios.post('https://demos.justlaravel.com/integrate-passport-laravel-api/oauth/token', {
          client_id:  client_id,
          client_secret: 'client_secret',
          grant_type: 'password',
          username: 'email',
          password: 'password',
          scope: '*',   
        })
        .then(response => { 
              this.access_token = response['data']['access_token'];
              this.get_users_data()
          })
        .catch(response => {
            console.log(response)
        });
      },

      get_users_data: function(){
        axios.get('https://demos.justlaravel.com/integrate-passport-laravel-api/api/users',{
          headers: {
             Authorization: 'Bearer ' + this.access_token
           }
        })
        .then(response => { 
              this.user_data = response['data'];
              return this.user_data;     
          })
        .catch(response => {
            console.log(response)
        });
      }
    },
    mounted(){
      this.get_data()
    }
  }

</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
