<template>
  <div v-if="author">
    <h2>{{ author.user.firstName }}</h2>
    <a :href="author.website" target="_blank" rel="noopener noreferrer"
      >Website</a
    >
    <p>{{ author.bio }}</p>

    <h3>Posts by {{ author.user.firstName }}</h3>
    <PostList :posts="author.postSet" :showAuthor="false" />
  </div>
</template>

<script>
import PostList from "@/components/PostList";
import gql from "graphql-tag";
import { useQuery } from "@vue/apollo-composable";
import { useRoute } from "vue-router";
import { computed } from "vue";

const AUTHOR_INFO = gql`
  query ($username: String!) {
    authorByUsername(username: $username) {
      website
      bio
      user {
        firstName
        lastName
        username
      }
      postSet {
        title
        subtitle
        publishDate
        published
        metaDescription
        slug
        tags {
          name
        }
      }
    }
  }
`;

export default {
  name: "Author",
  components: {
    PostList,
  },
  setup() {
    const route = useRoute();
    const { result, loading, error } = useQuery(AUTHOR_INFO, {
      username: route.params.username,
    });
    const author = computed(() => result.value?.authorByUsername ?? null);
    return {
      author,
      result,
      loading,
      error,
    };
  },
};
</script>