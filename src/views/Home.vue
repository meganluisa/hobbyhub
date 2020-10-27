<template>
<div class="home">
	<div id="banner-wrapper">
		<div id="banner" class="box container">
      <img v-if="status" v-bind:src="`https://http.cat/${status}`" alt ="">
		<form v-on:submit.prevent="createPost()">	
      <div class="row">
				<div class="col-7 col-12-medium">
					<h2>Meg's Music Gallery</h2>
               <ul>
             <li class="text-danger" v-for="error in errors" v-bind:key="error.id">{{ error }}</li>
           </ul>
           <div class="form-group">
             <label>Song title:</label> 
             <input type="text" class="form-control" v-model="title">
           </div>
           <div class="form-group">
             <label>Artist:</label> 
             <input type="text" class="form-control" v-model="body">
           </div>
           <div class="form-group">
             <label>Album image url:</label> 
             <input type="text" class="form-control" v-model="image">
           </div>
					<p> {{ message }}</p>
				</div>
				<div class="col-5 col-12-medium">
					<ul>
						<li><input type="submit" class="button large icon solid fa-arrow-circle-right" value="Add Post"></li>
						<li><a v-bind:href="'/about'" class="button alt large icon solid fa-question-circle">More info</a></li>
					</ul>
				</div>
			</div>
    </form>
		</div>
	</div>
  <!-- Conent -->
  <div id="features-wrapper">
	<div class="container">
    <div>
      Search jams: <input type="text" v-model="searchFilter" list="post-titles">
        <datalist id="post-titles">
          <option v-for="post in posts" v-bind:key="post.id">{{ post.title }}</option>
        </datalist>
    </div>
    <h3 style="text-align: center;">All Jams</h3>
		<div class="row">
			<div class="col-4 col-12-medium" v-for="post in filterBy(posts, searchFilter, 'title', 'body')" v-bind:key="post.id">
				<!-- Box -->
					<section class="box feature" >
						<a href="#" class="image featured"><img v-bind:src="post.image" alt="" /></a>
						<div class="inner">
							<header>
								<h2>{{ post.title }}</h2>
								<p><a v-bind:href="`/posts/${post.id}`">More</a></p>
							</header>
							<p>{{ post.body }}</p>
						</div>
					</section>
			</div>
		</div>
	</div>
	</div>
</div>
</template>

<style>
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  name: "PostsIndex",
  data: function() {
    return {
      message: "It Must Have Been the Roses...",
      searchFilter: "",
      posts: [],
      title: "",
      body: "",
      image: "",
      errors: [],
      status: "",
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
