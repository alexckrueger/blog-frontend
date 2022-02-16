<script>
export default {
  data: function () {
    return {
      message: "Hello Actualize Classmates!",
      favoriteLetter: "",
      favoriteLetterResponse: "",
    };
  },
  created: function () {},
  methods: {
    insertFavoriteLetter: function () {
      this.favoriteLetterResponse = "";
      if (this.favoriteLetter === "~") {
        this.favoriteLetterResponse = "~~~~~~~ Wow! What a coincidence! ~~~~~~~";
      } else {
        this.favoriteLetterResponse = `${this.favoriteLetter} is a very strange favorite letter...`;
      }
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <p>My favorite letter is ~</p>
      <div>
        What is yours?
        <input type="text" v-model="favoriteLetter" />
        <button v-if="!favoriteLetterResponse" v-on:click="insertFavoriteLetter">Enter Favorite Letter</button>
        <transition-group
          appear
          enter-active-class="animate__animated animate__bounceInDown"
          leave-active-class="animate__animated animate__bounceOutUp"
        >
          <p v-if="favoriteLetterResponse">{{ favoriteLetterResponse }}</p>
        </transition-group>
        <button v-if="favoriteLetterResponse" v-on:click="favoriteLetterResponse = ''">
          Wait, I change my mind...
        </button>
      </div>
    </div>
  </div>
</template>

<style></style>
