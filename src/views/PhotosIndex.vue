<script>
import axios from "axios";
export default {
  data: function () {
    return {
      photos: [],
    };
  },
  created: function () {
    this.indexPhotos();
  },
  methods: {
    indexPhotos: function () {
      axios.get("/photos").then((response) => {
        console.log("photos index", response);
        this.photos = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="photos-index">
    <h1>All Photos</h1>
    <div v-for="photo in photos" v-bind:key="photo.id">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" v-bind:alt="photo.name" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
      <router-link v-bind:to="`/photos/${photo.id}`">More details</router-link>
    </div>
  </div>
</template>
