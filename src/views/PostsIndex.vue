<template>

  <div class="posts-index">
    <h1>All Posts</h1>
    <div>
      Search posts: <input type="text" v-model="searchFilter" list="post-titles">
        <datalist id="post-titles">
          <option v-for="post in posts" v-bind:key="post.id">{{ post.title }}</option>
        </datalist>
    </div>
    <div v-for="post in filterBy(posts, searchFilter, 'title', 'body')" v-bind:key="post.id">
        <h2>Title: {{ post.title }}</h2>
        <p>Body: {{ post.body }}</p> 
        <a v-bind:href="`/posts/${post.id}`">More</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  name: "PostsIndex",
  data: function() {
    return {
      searchFilter: "",
      posts: [],
    };
  },
  created: function() {
      this.indexPosts();
  },
  methods: {
      indexPosts: function() {
          axios.get("/api/posts/").then((response) => {
              console.log("All posts:", response.data);
              this.posts = response.data;
          });
      },
  },
};
</script>
