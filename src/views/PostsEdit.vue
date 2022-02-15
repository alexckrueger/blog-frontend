<script>
import axios from "axios";

export default {
  data: function () {
    return {
      post: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.post = response.data;
    });
  },
  methods: {
    updatePost: function () {
      axios
        .patch(`/posts/${this.$route.params.id}`, this.post)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/posts/${this.post.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(error.response.data.errors);
          if (error.response.statusText === "Unauthorized") {
            this.errors = ["Please sign in before doing that action."];
          }
        });
    },
    deletePost: function () {
      if (confirm("Are you sure???")) {
        axios
          .delete(`/posts/${this.$route.params.id}`)
          .then((response) => {
            console.log(response.data);
            this.$router.push("/posts");
          })
          .catch((error) => {
            if (error.response.statusText === "Unauthorized") {
              this.errors = ["Please sign in before doing that action."];
            }
          });
      }
    },
  },
};
</script>

<template>
  <div class="posts-edit" v-if="post.owner">
    <button @click="$router.go(-1)">Go Back</button>
    <h1>EDIT POST</h1>
    <ul>
      <li id="red" v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
    <img :src="post.image" :alt="post.title" />
    <div>
      <p>
        Title:
        <input type="text" v-model="post.title" />
      </p>
      <p>
        Body:
        <input type="text" v-model="post.body" />
      </p>
      <p>
        Image Url:
        <input type="text" v-model="post.image" />
      </p>
      <button v-on:click="updatePost()">Submit</button>
      <br />
      <br />
      <button id="delete" v-on:click="deletePost()">DELETE POST!!!</button>
    </div>
  </div>
</template>

<style>
#red {
  color: red;
  font-weight: bolder;
  font-size: large;
}
#delete {
  color: darkred;
  font-size: xx-large;
  background-color: darkgray;
  border: 5px solid;
  width: 400px;
  height: 80px;
  font-family: Trebuchet;
  font-weight: bolder;
}
</style>
