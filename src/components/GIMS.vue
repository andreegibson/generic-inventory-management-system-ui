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
          <input type="text" id="searchCriteria" name="searchCriteria">
          <b-dropdown variant="primary" text="Search By" class="m-2">
            <b-dropdown-item v-on:click="readItem('name')">Name</b-dropdown-item>
            <b-dropdown-item v-on:click="readItem('category')">Category</b-dropdown-item>
            <b-dropdown-item v-on:click="readItem('type')">Type</b-dropdown-item>
            <b-dropdown-item v-on:click="readItem('brand')">Brand</b-dropdown-item>
          </b-dropdown>
        </div>
        <div v-if="items && !items.error" class="container overflow-scroll">
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
        <div v-if="items && items.error">
          <b-alert v-model="showAlert" variant="warning" dismissible>{{ items.error }}</b-alert>
        </div>
      </div>

      <div class="container mh-25">
        <h3>Add an item to the Item Database</h3>
        <div class="inputs">
          <label for="newItemName"/>
          <input type="text" id="newItemName" name="newItemName">
        </div>
        <!--        <div v-if="items && !items.error" class="container overflow-scroll">-->
        <!--          <table class="table table-striped table-bordered">-->
        <!--            <thead>-->
        <!--            <tr>-->
        <!--              <th>ID</th>-->
        <!--              <th>Name</th>-->
        <!--              <th>Category</th>-->
        <!--              <th>Type</th>-->
        <!--              <th>Brand</th>-->
        <!--              <th>Description</th>-->
        <!--            </tr>-->
        <!--            </thead>-->
        <!--            <tbody>-->
        <!--            <tr v-for="item in items" :key="item.id">-->
        <!--              <td>{{ item.id }}</td>-->
        <!--              <td>{{ item.name }}</td>-->
        <!--              <td>{{ item.category }}</td>-->
        <!--              <td>{{ item.type }}</td>-->
        <!--              <td>{{ item.brand }}</td>-->
        <!--              <td>{{ item.description }}</td>-->
        <!--            </tr>-->
        <!--            </tbody>-->
        <!--          </table>-->
        <!--        </div>-->
        <!--        <div v-if="items && items.error">-->
        <!--          <b-alert v-model="showAlert" variant="warning" dismissible>{{ items.error }}</b-alert>-->
        <!--        </div>-->
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    msg: String
  },
  data() {
    return {
      showAlert: false,
      items: null
    }
  },
  methods: {
    readItem: function (searchParam) {
      let searchCriteria = document.getElementById('searchCriteria').value;
      let vm = this;
      axios
          .get('http://localhost:8081/items/' + searchParam + '/' + searchCriteria)
          .then(function (response) {
            vm.items = response.data;
            vm.showAlert = false;
            if (vm.items.length === 0) {
              vm.items.error = 'No items found with ' + searchParam + ' ' + searchCriteria + '.';
              vm.showAlert = true;
            }
          })
          .catch(function (error) {
            vm.items.error = 'An error occurred' + error;
            vm.showAlert = true;
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
