<script>
import axios from "axios";
import dayjs from "dayjs";
dayjs().format();
var relativeTime = require("dayjs/plugin/relativeTime");
dayjs.extend(relativeTime);

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
      nameFilter: "",
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log(response.data);
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
    relativeTime: function (date) {
      return dayjs().to(dayjs(date));
    },
    titleFilter: function () {
      return this.posts.filter((post) => {
        return post.title.toLowerCase().includes(this.nameFilter);
      });
    },
  },
};
</script>

<template>
  <div class="posts">
    <button v-on:click="doNotClick()">{{ buttonText[buttonIndex] }}</button>
    <p>
      Search:
      <input type="text" v-model="nameFilter" />
    </p>
    <div v-for="post in titleFilter()" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <small>Created {{ relativeTime(post.created_at) }}</small>
      <br />
      <router-link :to="`/posts/${post.id}`"><img :src="post.image" :alt="post.title" /></router-link>
    </div>
  </div>
</template>
