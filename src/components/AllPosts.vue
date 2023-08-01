<template>
  <div>
    <h2>Recent posts</h2>
    <PostList v-if="result" :posts="result.allPosts" />
  </div>
</template>

<script>
import gql from "graphql-tag";
import { useQuery } from "@vue/apollo-composable";
import PostList from "@/components/PostList";

const ALL_POSTS = gql`
  query {
    allPosts {
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
  name: "AllPosts",
  components: {
    PostList,
  },
  setup() {
    const { result, loading, error } = useQuery(ALL_POSTS);
    return {
      result,
      loading,
      error,
    };
  },
};
</script>
