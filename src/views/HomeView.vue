<template>
  <div class="home">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length" class="layout">
      <PostList :posts="posts" @delete="handleDeletePost" />
      <TagCloud :posts="posts" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>
<script setup >
import { provide, inject } from "vue";
import { ref } from "vue";
import PostList from "../components/PostList.vue";
import TagCloud from "../components/TagCloud.vue";
import getPosts from "../composables/getPosts";
import Spinner from "../components/Spinner.vue";

const { posts, error, load } = getPosts();
const handleDeletePost = (id) => {
  posts.value = posts.value.filter((post) => post.id !== id);
};
provide("handleDeletePost", handleDeletePost);

load();
</script>
<style >
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
</style>
