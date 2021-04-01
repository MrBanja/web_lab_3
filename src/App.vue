<template>
  <div id="app">
    <div class="log-in">
      <div class="container-fluid mt-4">
        <div class="row">
          <div class="col"></div>
          <div class="col justify-content-center">
            <table class="table table-striped">
              <thead>
              <tr>
                <th>Name</th>
                <th>Age</th>
                <th>uni</th>
                <th>Update</th>
                <th>Delete</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="(column, index) in some_info" :key="index">
                <th>{{column.name}}</th>
                <th>{{column.age}}</th>
                <th>{{column.uni}}</th>
                <th><button type="button" class="btn btn-primary" @click="setUpdating(column)">Update</button></th>
                <th><button type="button" class="btn btn-danger" @click="deleteUser(column.id)">Delete</button></th>
              </tr>
              </tbody>
            </table>
            <button type="button" class="btn btn-primary" @click="doCall()">Get Fresh data</button>

            <UpdateForm @update-emit="doCall" v-if="updating.state" :user="updating.user" :key="key"/>
            <CreateForm @update-emit="doCall"/>

          </div>
          <div class="col"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import UpdateForm from "@/components/UpdateForm";
import CreateForm from "@/components/CreateForm";

export default {
  name: 'App',
  components: {
    UpdateForm,
    CreateForm
  },
  data(){
    return {
      key: 0,
      updating: {
        state: false,
        user: {}
      },
      some_info: [],
    }
  },
  methods: {
    doCall(){
      axios
          .get('http://localhost:5000/users')
          .then(response => (this.some_info = response.data))
    },
    deleteUser(id){
      axios
          .delete('http://localhost:5000/users/' + id)
          .then( () => (this.doCall()))
    },

    setUpdating(column){
      this.key++;
      if (this.key > 10)
        this.key = 0

      this.updating.state = true;
      this.updating.user.id = column.id;
      this.updating.user.name = column.name;
      this.updating.user.age = column.age
      this.updating.user.uni = column.uni
    },
    removeUpdating(){
      this.updating.state = false;
      this.updating.user = {};
    }
  },
  mounted() {
    axios
        .get('http://localhost:5000/users')
        .then(response => (this.some_info = response.data))
  }
}
</script>

<style>
@import 'https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css';
</style>
