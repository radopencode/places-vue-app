<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: [],
      newPlaceParams: {},
      editPlaceParams: {},
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPhotos();
  },
  methods: {
    indexPlace: function () {
      axios.get("/places").then((response) => {
        console.log("places index", response);
        this.places = response.data;
      });
    },
    createPlace: function () {
      axios
        .post("/places", this.newPlaceParams)
        .then((response) => {
          console.log("places create", response);
          this.places.push(response.data);
          this.newPlaceParams = {};
        })
        .catch((error) => {
          console.log("places create error", error.response);
        });
    },
    showPlaces: function (place) {
      this.currentPlace = place;
      this.editPlaceParams = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      axios
        .patch("/places/" + place.id, this.editPlaceParams)
        .then((response) => {
          console.log("places update", response);
          this.currentPlace = {};
        })
        .catch((error) => {
          console.log("places update error", error.response);
        });
    },
    destroyPlace: function (place) {
      axios.delete("/places/" + place.id).then((response) => {
        console.log("places destroy", response);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Place</h1>
    <div>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
      Address:
      <input type="text" v-model="newPlaceParams.width" />
      <button v-on:click="createPlace()">Create New Place</button>
    </div>
    <h1>All Places</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>{{ place.name }}</h2>
      <img v-bind:src="place.url" v-bind:alt="place.name" />
      <p>Address: {{ photo.address }}</p>
      <button v-on:click="showPlace(place)">More info</button>
    </div>
    <dialog id="place-details">
      <form method="dialog">
        <h1>Place info</h1>
        <p>
          Name:
          <input type="text" v-model="editPlaceParams.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="editPlaceParams.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">Update</button>
        <button v-on:click="destroyPlace(currentPlace)">Destroy Place</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
