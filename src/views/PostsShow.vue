<script>
import axios from "axios";
import dayjs from "dayjs";
dayjs().format();
var relativeTime = require("dayjs/plugin/relativeTime");
dayjs.extend(relativeTime);

export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log("hip hip hoorayy");
      this.post = response.data;
    });
  },
  methods: {
    relativeTime: function (date) {
      return dayjs().to(dayjs(date));
    },
  },
};
</script>

<template>
  <div class="posts-show">
    <button @click="$router.go(-1)">Go Back</button>
    <h2>{{ post.title }}</h2>
    <small>Created {{ relativeTime(post.created_at) }}</small>
    <br />
    <img :src="post.image" :alt="post.title" />
    <p>{{ post.body }}</p>
    <router-link v-if="post.owner" :to="`/posts/${post.id}/edit`"><button>Edit Post</button></router-link>
  </div>
</template>
