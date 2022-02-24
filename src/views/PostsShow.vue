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
    <div class="container">
      <button @click="$router.go(-1)" type="button" class="btn btn-secondary mt-2 mb-4">&lt;-- Go Back</button>
      <div class="row">
        <div class="col-3 w-auto"><img :src="post.image" :alt="post.title" /></div>
        <div class="col">
          <h2>{{ post.title }}</h2>
          <p>{{ post.body }}</p>
          <small class="card-footer">Created {{ relativeTime(post.created_at) }}</small>
        </div>
      </div>

      <router-link v-if="post.owner" :to="`/posts/${post.id}/edit`"><button>Edit Post</button></router-link>
    </div>
  </div>
</template>
