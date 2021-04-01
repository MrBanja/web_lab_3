<template>
  <div class="hello">
    <form @submit.prevent="updateUser()">
      <div class="form-group">
        <label for="name">Name</label>
        <input type="text" class="form-control" v-model=user.name id="name">
      </div>
      <div class="form-group">
        <label for="age">Age:</label>
        <input type="number" class="form-control" v-model="user.age" id="age">
      </div>
      <div class="form-group">
        <label for="uni">Uni:</label>
        <input type="text" class="form-control" v-model="user.uni" id="uni">
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'UpdateForm',
  emits: ['update-emit'],
  props: {
    user: Object
  },
  methods: {
    updateUser: function () {
      axios
          .put("http://localhost:5000/users/" + this.user.id, this.user)
          .then(() => {
            this.$emit('update-emit');
          } )
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
