<script>
import axios from "axios";
export default {
  data: function () {
    return {
      photos: [],
      newPhotoParams: {},
      editPhotoParams: {},
      currentPhoto: {},
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
    createPhoto: function () {
      axios
        .post("/photos", this.newPhotoParams)
        .then((response) => {
          console.log("photos create", response);
          this.photos.push(response.data);
          this.newPhotoParams = {};
        })
        .catch((error) => {
          console.log("photos create error", error.response);
        });
    },
    showPhoto: function (photo) {
      this.currentPhoto = photo;
      this.editPhotoParams = photo;
      document.querySelector("#photo-details").showModal();
    },
    updatePhoto: function (photo) {
      axios
        .patch("/photos/" + photo.id, this.editPhotoParams)
        .then((response) => {
          console.log("photos update", response);
          this.currentPhoto = {};
        })
        .catch((error) => {
          console.log("photos update error", error.response);
        });
    },
    destroyPhoto: function (photo) {
      axios.delete("/photos/" + photo.id).then((response) => {
        console.log("photos destroy", response);
        var index = this.photos.indexOf(photo);
        this.photos.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Photo</h1>
    <div>
      Name:
      <input type="text" v-model="newPhotoParams.name" />
      Width:
      <input type="text" v-model="newPhotoParams.width" />
      Height:
      <input type="text" v-model="newPhotoParams.height" />
      Url:
      <input type="text" v-model="newPhotoParams.url" />
      <button v-on:click="createPhoto()">Create Photo</button>
    </div>
    <h1>All Photos</h1>
    <div v-for="photo in photos" v-bind:key="photo.id">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" v-bind:alt="photo.name" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
      <button v-on:click="showPhoto(photo)">More info</button>
    </div>
    <dialog id="photo-details">
      <form method="dialog">
        <h1>Photo info</h1>
        <p>
          Name:
          <input type="text" v-model="editPhotoParams.name" />
        </p>
        <p>
          Width:
          <input type="text" v-model="editPhotoParams.width" />
        </p>
        <p>
          Height:
          <input type="text" v-model="editPhotoParams.height" />
        </p>
        <p>
          Url:
          <input type="text" v-model="editPhotoParams.url" />
        </p>
        <button v-on:click="updatePhoto(currentPhoto)">Update</button>
        <button v-on:click="destroyPhoto(currentPhoto)">Destroy Photo</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
