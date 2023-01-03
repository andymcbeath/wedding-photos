<script>
import axios from "axios";
export default {
  data: function () {
    return {
      photo: {},
    };
  },
  created: function () {
    axios.get("/photos/" + this.$route.params.id).then((response) => {
      console.log("photos show", response);
      this.photo = response.data;
    });
  },
  methods: {
    destroyPhoto: function (photo) {
      axios.delete("/photos/" + photo.id).then((response) => {
        console.log("photos destroy", response);
        this.$router.push("/photos");
      });
    },
  },
};
</script>

<template>
  <div class="photos-show">
    <h2>{{ photo.name }}</h2>
    <img v-bind:src="photo.url" v-bind:alt="photo.name" />
    <p>Width: {{ photo.width }}</p>
    <p>Height: {{ photo.height }}</p>
    <router-link v-bind:to="`/photos/${photo.id}/edit`">Edit photo</router-link>
    <button v-on:click="destroyPhoto(photo)">Destroy photo</button>
    <router-link to="/photos">Back to all photos</router-link>
  </div>
</template>
