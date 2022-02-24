<script>
export default {
  data: function () {
    return {
      isLoggedIn: !!localStorage.jwt,
      flashMessage: null,
    };
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
      this.flashMessage = localStorage.flashMessage;
      localStorage.removeItem("flashMessage");
    },
  },
};
</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark bg-gradient">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">Blog Yeeeee</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link active" to="/about">About</router-link>
          </li>
          <li class="nav-item">
            <router-link v-if="!isLoggedIn" to="/signup" class="nav-link">Signup</router-link>
          </li>
          <li class="nav-item">
            <router-link v-if="!isLoggedIn" to="/login" class="nav-link">Login</router-link>
          </li>
          <li class="nav-item">
            <router-link v-if="isLoggedIn" to="/logout" class="nav-link">Logout</router-link>
          </li>
          <li class="nav-item">
            <router-link to="/posts" class="nav-link">Posts</router-link>
          </li>
          <li class="nav-item">
            <router-link v-if="isLoggedIn" to="/posts/new" class="nav-link">New Post</router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <div v-if="flashMessage">
    {{ flashMessage }}
    <button v-on:click="flashMessage = ''">Dismiss</button>
  </div>
  <router-view />
</template>

<style>
body {
  background-image: url("../public/img/beanstalk.png");
}
</style>
