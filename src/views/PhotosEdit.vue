<script>
import axios from "axios";
export default {
  data: function () {
    return {
      photo: {},
      editPhotoParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/photos/" + this.$route.params.id).then((response) => {
      console.log("photos show", response);
      this.photo = response.data;
      this.editPhotoParams = this.photo;
    });
  },
  methods: {
    updatePhoto: function (photo) {
      axios
        .patch("/photos/" + photo.id, this.editPhotoParams)
        .then((response) => {
          console.log("photos update", response);
          this.$router.push("/photos");
        })
        .catch((error) => {
          console.log("photos update error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="photos-edit">
    <h1>Edit Photo</h1>
    <form v-on:submit.prevent="updatePhoto(photo)">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="editPhotoParams.name" />
      Width:
      <input type="text" v-model="editPhotoParams.width" />
      Height:
      <input type="text" v-model="editPhotoParams.height" />
      Url:
      <input type="text" v-model="editPhotoParams.url" />
      <input type="submit" value="Update" />
    </form>
  </div>
</template>
