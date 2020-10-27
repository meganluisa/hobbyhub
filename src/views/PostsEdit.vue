<template>
    <div class="posts-edit">
        <form v-on:submit.prevent="updatePost()">
            <h1>Posts edit</h1>
            <ul>
                <li class="text-danger" v-for="error in errors">{{ error }}</li>
            </ul>
            <div class="form-group">
                <label>title:</label>
                <input type="text" class="form-control" v-model="post.title">
            </div>
        </form>
    </div>

</template>