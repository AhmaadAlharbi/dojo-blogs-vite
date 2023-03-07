<template>
  <div class="home">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length">
      <PostList :posts="postsTag" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<script setup>
import getPosts from "../composables/getPosts";
import PostList from "../components/PostList.vue";
import Spinner from "../components/Spinner.vue";
import { computed } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const { posts, error, load } = getPosts();
const postsTag = computed(() => {
  return posts.value.filter((post) => post.tags.includes(route.params.tag));
});
load();
</script>

<style>
</style>