<template>
  <div class="gims">
    <div class="container">
      <h1>{{ msg }}</h1>
      <p>
        An application for managing an inventory database.
      </p>
      <div class="container mh-25">
        <h3>Retrieve item(s) from the Item Database</h3>
        <div class="inputs">
          <label for="getItemName">Item name:</label>
          <input type="text" id="getItemName" name="getItemName">
          <button v-on:click="readItem">Lookup Item</button>
        </div>
        <div class="container overflow-scroll">
          <table class="table table-striped table-bordered">
            <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Category</th>
              <th>Type</th>
              <th>Brand</th>
              <th>Description</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="item in items" :key="item.id">
              <td>{{ item.id }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.category }}</td>
              <td>{{ item.type }}</td>
              <td>{{ item.brand }}</td>
              <td>{{ item.description }}</td>
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

export default {
  el: '#GIMS',
  props: {
    msg: String,
    items: JSON
  },
  methods: {
    readItem: function () {
      this.getItemResults = 'Loading...';
      let itemName = document.getElementById('getItemName').value;
      let vm = this;
      axios
          .get('http://localhost:8081/items/name/' + itemName)
          .then(function (response) {
            vm.items = response.data;
          })
          .catch(function (error) {
            vm.status = 'An error occurred' + error;
          })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
