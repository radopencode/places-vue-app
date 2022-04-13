<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newPlaceParams: {},
      errors: [],
    };
  },
  created: function () {},
  methods: {
    createPlace: function () {
      axios
        .post("/places", this.newPlaceParams)
        .then((response) => {
          console.log("places create", response);
          this.$router.push("/places");
        })
        .catch((error) => {
          console.log("places create error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="places-new">
    <h1>New Place</h1>
    <form v-on:submit.prevent="createPlace()">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
      Width:
      <input type="text" v-model="newPlaceParams.width" />
      <input type="submit" value="Create" />
    </form>
  </div>
</template>
