<script>
import axios from "axios";
export default {
  data: function () {
    return {
      places: [],
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/places").then((response) => {
        console.log("places index", response);
        this.places = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="places-index">
    <h1>All Places</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>{{ place.name }}</h2>
      <img v-bind:src="place.url" v-bind:alt="place.name" />
      <p>Address: {{ photo.address }}</p>
      <router-link v-bind:to="`/places/${place.id}`">More details</router-link>
    </div>
  </div>
</template>
