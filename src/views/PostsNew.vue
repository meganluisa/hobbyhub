<template>
  <div class="posts-new">
    <img v-if="status" v-bind:src="`https://http.cat/${status}`" alt ="">
    <form v-on:submit.prevent="createPost()">
      <h1>Posts new</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error.id">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>title:</label> 
        <input type="text" class="form-control" v-model="title">
      </div>
      <div class="form-group">
        <label>body:</label> 
        <input type="text" class="form-control" v-model="body">
      </div>
      <div class="form-group">
        <label>image:</label> 
        <input type="text" class="form-control" v-model="image">
      </div>
      <input type="submit" class="btn btn-primary" value="Submit">
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "PostsNew",
  data: function () {
    return {
      title: "",
      body: "",
      image: "",
      errors: [],
      status: "",
    };
  },
  methods: {
    createPost: function () {
      var params = {
        title: this.title,
        body: this.body,
        image: this.image,
      };
      axios
        .post("/api/posts", params)
        .then((response) => {
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          this.status = error.response.status;
        });
    },
  },
};
</script>