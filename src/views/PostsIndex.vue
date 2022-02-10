<script>import axios from "axios";

export default {
  data: function () {
    return {
      posts: [],
      buttonText: [
        "Don't Click Me",
        "Don't Click Me",
        "Don't Click Me",
        "Don't Click Me",
        "I can't believe you've done this...",
        "Cut it out",
        "Why would you do this",
        "seriously",
        "omg...",
        "I'm going to pretend like this never happened",
      ],
      buttonIndex: 0,
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log("hip hip hoorayy");
      this.posts = response.data;
    });
  },
  methods: {
    doNotClick: function () {
      this.buttonIndex += 1;
      if (this.buttonIndex === 10) {
        this.buttonIndex = 0;
      }
    },
  },
};
</script>

<template>
  <div class="posts">
    <button v-on:click="doNotClick()">{{ buttonText[buttonIndex] }}</button>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <img :src="post.image" :alt="post.title" />
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>
