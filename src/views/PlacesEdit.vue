<script>
import axios from "axios";
export default {
  data: function () {
    return {
      place: {},
      editPlaceParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/places/" + this.$route.params.id).then((response) => {
      console.log("places show", response);
      this.place = response.data;
      this.editPlaceParams = this.place;
    });
  },
  methods: {
    updatePlace: function (place) {
      axios
        .patch("/places/" + place.id, this.editPlaceParams)
        .then((response) => {
          console.log("places update", response);
          this.$router.push("/places");
        })
        .catch((error) => {
          console.log("places update error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="places-edit">
    <h1>Edit Place</h1>
    <form v-on:submit.prevent="updatePlace(place)">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="editPlaceParams.name" />
      Address:
      <input type="text" v-model="editPlaceParams.address" />
      Url:
      <input type="text" v-model="editPlaceParams.url" />
      <input type="submit" value="Update" />
    </form>
  </div>
</template>
