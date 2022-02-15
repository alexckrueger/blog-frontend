<script>
import axios from "axios";

export default {
  created: function () {
    delete axios.defaults.headers.common["Authorization"];
    localStorage.removeItem("jwt");
    localStorage.setItem("flashMessage", "You've logged out.");
    this.$router.push("/");
  },
};
</script>

<template>
  <div class="logout"></div>
</template>
