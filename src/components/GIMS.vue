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
        <b-card bg-variant="light">
          <b-form @submit.prevent>
            <b-form-group
                id="addItem-name-group"
                label="Name:"
                label-for="addItem-name"
                label-align-sm="left"
                description="The name of the item."
            >
              <b-form-input
                  id="addItem-name"
                  v-model="form.name"
                  type="String"
                  placeholder="Item name"
                  required
              ></b-form-input>
            </b-form-group>

            <b-form-group
                id="addItem-category-group"
                label="Category:"
                label-for="addItem-category"
                label-align-sm="left"
                description="The category of the item.  (e.g. A/C, refrigeration, electrical, etc)"
            >
              <b-form-input
                  id="addItem-category"
                  v-model="form.category"
                  type="String"
                  placeholder="Item category"
                  required
              ></b-form-input>
            </b-form-group>

            <b-form-group
                id="addItem-type-group"
                label="Type:"
                label-for="addItem-type"
                label-align-sm="left"
                description="The type of the item. (e.g. maintenance, components, etc)"
            >
              <b-form-input
                  id="addItem-type"
                  v-model="form.type"
                  type="String"
                  placeholder="Item type"
                  required
              ></b-form-input>
            </b-form-group>

            <b-form-group
                id="addItem-brand-group"
                label="Brand:"
                label-for="addItem-brand"
                label-align-sm="left"
                description="The brand of the item. (e.g. Trane, Lennox, American Standard)"
            >
              <b-form-input
                  id="addItem-brand"
                  v-model="form.brand"
                  type="String"
                  placeholder="Item brand"
                  required
              ></b-form-input>
            </b-form-group>

            <b-form-group
                id="addItem-description-group"
                label="Description:"
                label-for="addItem-description"
                label-align-sm="left"
                description="The description of the item."
            >
              <b-form-input
                  id="addItem-description"
                  v-model="form.description"
                  type="String"
                  placeholder="Description"
                  required
              ></b-form-input>
            </b-form-group>

            <b-form-group
                id="addItem-imageURL-group"
                label="Image URL:"
                label-for="addItem-imageURL"
                label-align-sm="left"
                description="The URL of the picture of the item."
            >
              <b-form-input
                  id="addItem-imageURL"
                  v-model="form.imageURL"
                  type="String"
                  placeholder="http://image.url"
              ></b-form-input>
            </b-form-group>
            <b-button @click="addItem" variant="primary">Submit</b-button>
            <b-button @click="resetAddItem" variant="danger">Reset</b-button>
          </b-form>
        </b-card>

        <div>
          <b-modal id="addItem-success-modal" title="Add Item Success">
            <p class="my-4">Your new item, {{ newItemName }}, was added to the database.</p>
          </b-modal>
        </div>

        <div>
          <b-modal id="addItem-failure-modal" title="Add Item Success">
            <p class="my-4">Failed to add item to the db: <br/>{{ addItemError }}</p>
          </b-modal>
        </div>
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
      items: null,
      form:
          {
            name: '',
            category: '',
            type: '',
            brand: '',
            description: '',
            imageURL: '',

          }
      ,
      newItemName: null,
      addItemError: null
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
    },
    addItem: function () {
      let vm = this;
      let name = document.getElementById('addItem-name').value;
      let category = document.getElementById('addItem-category').value;
      let type = document.getElementById('addItem-type').value;
      let brand = document.getElementById('addItem-brand').value;
      let description = document.getElementById('addItem-description').value;
      let imageURL = document.getElementById('addItem-imageURL').value;
      axios
          .post("http://localhost:8081/items/",
              {
                "name": name,
                "category": category,
                "type": type,
                "brand": brand,
                "description": description,
                "imageUrl": imageURL
              })
          .then(function (response) {
            vm.newItemName = response.data.name
            vm.$refs['addItem-success-modal'].show()
          })
          .catch(function (error) {
            vm.addItemError = error
            vm.$refs['addItem-failure-modal'].show()
          });
    },
    resetAddItem: function () {
      this.form = {
        name: '',
        category: '',
        type: '',
        brand: '',
        description: '',
        imageURL: '',

      }
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
