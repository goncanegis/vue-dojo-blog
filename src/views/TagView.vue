<template>
  <div class="tag">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length" class="layout">
      <PostList :posts="postsWithTag" />
      <TagCloud :posts="posts" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<script>
import getPosts from "../composables/getPosts.js";
import Spinner from "../components/Spinner.vue";
import PostList from "../components/PostList.vue";
import { useRoute } from "vue-router";
import { computed } from "vue";
import TagCloud from "@/components/TagCloud.vue";

export default {
  components: {
    PostList,
    Spinner,
    TagCloud,
  },
  setup() {
    const route = useRoute();
    const { posts, error, load } = getPosts();

    load();

    const postsWithTag = computed(() => {
      return posts.value.filter((post) => post.tags.includes(route.params.tag));
    });

    return { posts, error, postsWithTag };
  },
};
</script>

<style></style>
