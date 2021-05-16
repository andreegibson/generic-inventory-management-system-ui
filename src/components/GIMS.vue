<template>
  <div class="gims">
    <h1>{{ msg }}</h1>
    <p>
      Description text goes here.
    </p>
    <h3>Retrieve an Item from the Item Database</h3>
    <div class="inputs">
      <label for="getItemName">Item name:</label>
      <input type="text" id="getItemName" name="getItemName">
      <button v-on:click="readItem">Lookup Item</button>
    </div>
    <div>
      {{ getItemResults }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default{
  el: '#GIMS',
  props: {
    msg: String,
    getItemResults: JSON
  },
  methods: {
    readItem: function() {
      this.getItemResults = 'Loading...';
      let itemName = document.getElementById('getItemName').value;
      let vm = this;
      axios
          .get('http://localhost:8081/items/name/' + itemName)
          .then(function(response){
            vm.getItemResults = response.data;
          })
          .catch(function(error){
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
