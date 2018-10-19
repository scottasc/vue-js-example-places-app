<template>
  <div class="home">

    <div v-for="place in places">
      {{ place.name }}: {{ place.address }}
    </div>

    <div>
        <input v-model="newPlace.name"> Place name<br/>
        <input v-model="newPlace.address"> Address<br/>
        <button v-on:click="addPlace()">Add place</button>
    </div>


  </div>
</template>

<style>

body {
  background-color: lightblue;
}

body:hover {
    background-color: lightgreen;
    transition: background-color 800ms linear;
}

</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      places: [],
      newPlace: {name: "", address: ""}
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/places")
    .then(function(response) {
      this.places = response.data;
    }
    .bind(this));
  },
  methods: {
    addPlace: function() {
        var params = {
                      name: this.newPlace.name,
                      address: this.newPlace.address
                     };

        axios
        .post("http://localhost:3000/api/places", params)
        .then(function(response) {
            this.places.push(response.data);
        }.bind(this));
        this.newPlace = {name: "", address: ""};
  }
},
  computed: {}
};
</script>
