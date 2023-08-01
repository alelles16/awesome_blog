<template>
  <div>
    <h2>Posts in #{{ $route.params.tag }}</h2>
    <PostList :posts="posts" v-if="posts" />
  </div>
</template>

<script>
import PostList from "@/components/PostList";
import gql from "graphql-tag";
import { useQuery } from "@vue/apollo-composable";
import { useRoute } from "vue-router";
import { computed } from "vue";

const POST_BY_TAG = gql`
  query ($tag: String!) {
    postsByTag(tag: $tag) {
      title
      subtitle
      publishDate
      published
      metaDescription
      slug
      author {
        user {
          username
          firstName
          lastName
        }
      }
      tags {
        name
      }
    }
  }
`;

export default {
  name: "PostsByTag",
  components: {
    PostList,
  },
  setup() {
    const route = useRoute();
    const { result, loading, error } = useQuery(POST_BY_TAG, {
      tag: route.params.tag,
    });
    const posts = computed(() => result.value?.postsByTag ?? null);

    return {
      posts,
      result,
      loading,
      error,
    };
  },
};
</script>