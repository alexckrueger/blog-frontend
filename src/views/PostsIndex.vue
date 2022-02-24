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
        return post.title.toLowerCase().includes(this.nameFilter.toLowerCase());
      });
    },
  },
};
</script>

<template>
  <div class="posts">
    <div class="container">
      <button
        v-on:click="doNotClick()"
        type="button"
        class="btn mt-2 btn-outline-danger p-2 position-relative top-0 start-50 translate-middle-x"
      >
        {{ buttonText[buttonIndex] }}
      </button>

      <div class="input-group mb-3 mt-3">
        <input type="text" class="form-control" v-model="nameFilter" list="postTitles" />
        <datalist id="postTitles">
          <option v-for="post in posts" v-bind:key="post.id">{{ post.title }}</option>
        </datalist>
        <button class="btn btn-outline-secondary" type="button" id="button-addon2">Search</button>
      </div>

      <div class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4">
        <div v-for="post in titleFilter()" v-bind:key="post.id" class="card mb-3" style="max-width: 540px">
          <div class="row g-0">
            <div class="col-md-4">
              <img :src="post.image" :alt="post.title" class="img-fluid rounded-start" />
            </div>
            <div class="col-md-8">
              <div class="card-body">
                <h5 class="card-title mb-2">{{ post.title }}</h5>
                <p class="text-truncate">{{ post.body }}</p>
                <router-link :to="`/posts/${post.id}`">
                  <button type="button" class="btn btn-outline-dark mb-2">View Post</button>
                </router-link>
                <div class="card-footer">
                  <small class="text-muted">Created {{ relativeTime(post.created_at) }}</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
