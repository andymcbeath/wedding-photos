<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newPhotoParams: {},
      errors: [],
    };
  },
  created: function () {},
  methods: {
    createPhoto: function () {
      axios
        .post("/photos", this.newPhotoParams)
        .then((response) => {
          console.log("photos create", response);
          this.$router.push("/photos");
        })
        .catch((error) => {
          console.log("photos create error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="photos-new">
    <h1>New Photo</h1>
    <form v-on:submit.prevent="createPhoto()">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="newPhotoParams.name" />
      Width:
      <input type="text" v-model="newPhotoParams.width" />
      Height:
      <input type="text" v-model="newPhotoParams.height" />
      Url:
      <input type="text" v-model="newPhotoParams.url" />
      <input type="submit" value="Create" />
    </form>
  </div>
</template>
