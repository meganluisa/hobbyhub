<template>
  <div class="posts-show">
    <h1>Posts</h1>
    <h2>{{ post.title }}</h2>
    <img v-bind:src="post.image" alt="">
    <p>Title: {{ post.title }}</p>
    <p>Body: {{ post.body }} </p>
  </div>
</template>
<style>
</style>
<script>
import axios from "axios";
export default {
  name: "PostsShow",
  data: function() {
    return {
      post: {},
    };
  },
  created: function() {
      this.showPost();
  },
  methods: {
      showPost: function() {
          axios.get("/api/posts/" + this.$route.params.id).then((response) => {
              console.log("One post:", response.data);
              this.post = response.data;
          });
      },
  },
};
</script>
