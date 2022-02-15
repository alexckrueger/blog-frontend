<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newPostParams: {},
      errors: [],
      errorStatus: "",
    };
  },
  methods: {
    createPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(error.response.data.errors);
          this.errorStatus = error.response.status;
          console.log(this.errorStatus);
          if (error.response.statusText === "Unauthorized") {
            this.errors = ["Please sign in before doing that action."];
          }
        });
    },
  },
};
</script>

<template>
  <div class="new-posts">
    <form v-on:submit.prevent="createPost()">
      <h1>New Post</h1>
      <ul>
        <li id="red" v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <img v-if="errorStatus" :src="`https://http.cat/${errorStatus}`" alt="" />
      <div>
        <label>Title:</label>
        <input type="text" v-model="newPostParams.title" />
        <br />
        <small class="danger" v-if="newPostParams.title && newPostParams.title.length < 10">
          Post title should be at least 10 characters
        </small>
        <small class="danger" v-if="newPostParams.title && newPostParams.title.length > 50">
          Post title should be less than 50 characters
        </small>
      </div>
      <br />
      <div>
        <label>Body:</label>
        <input type="text" v-model="newPostParams.body" />
        <br />
        <small v-if="newPostParams.body && newPostParams.body.length < 141">
          {{ 140 - newPostParams.body.length }} characters remaining
        </small>
        <small class="danger" v-if="newPostParams.body && newPostParams.body.length > 140">
          {{ newPostParams.body.length - 140 }} characters too many!
        </small>
      </div>
      <br />
      <div>
        <label>Image Url:</label>
        <input type="text" v-model="newPostParams.image" />
      </div>
      <br />
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<style>
#red {
  color: red;
  font-weight: bolder;
  font-size: large;
}
.danger {
  color: rgb(243, 56, 56);
}
</style>
