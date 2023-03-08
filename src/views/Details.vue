<template>
  <div v-if="error">{{ error }}</div>
  <div v-if="post" class="post">
    <h3>{{ post.title }}</h3>
    <p class="pre">{{ post.body }}</p>
    <span v-for="tag in post.tags" :key="tag"> #{{ tag }} </span>
    <button @click="deletePost" style="font-size: 24px">
      <i class="fa fa-trash-o" style="color: #ff8800"></i>
    </button>
  </div>
  <div v-else>
    <Spinner />
  </div>
</template>

<script >
import { ref } from "vue";
import getPost from "../composables/getPost";
import Spinner from "../components/Spinner.vue";
import { useRoute, useRouter } from "vue-router";
import { inject } from "vue";

export default {
  props: ["id"],
  components: { Spinner },
  setup(props) {
    const route = useRoute();
    const { post, error, load } = getPost(route.params.id);
    load();
    const handleDeletePost = inject("handleDeletePost");
    const router = useRouter();
    const deletePost = () => {
      fetch(`http://localhost:3000/posts/${props.id}`, {
        method: "DELETE",
      })
        .then(() => {
          // call the handleDeletePost function with the post ID
          handleDeletePost(props.id);
        })
        .catch((error) => {
          console.error("Error deleting post:", error);
        });
      router.push({ name: "Home" });
    };
    return { post, error, deletePost };
  },
};
</script>
 fetch(uri, { method: 'DELETE' })
        .then(() => this.$emit('delete', this.project.id))
        .catch(err => console.log(err))
<style scoped>
.post {
  max-width: 1200px;
  margin: 0 auto;
}
.post p {
  color: #444;
  line-height: 1.5em;
  margin-top: 40px;
}
.pre {
  white-space: pre-wrap;
}
button {
  background: white;
  border: 1px #ff8800 solid;
  color: black;
  border-radius: 20px;
  cursor: pointer;
}
</style>