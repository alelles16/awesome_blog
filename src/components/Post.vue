<template>
  <div class="post" v-if="post">
    <h2>{{ post.title }}: {{ post.subtitle }}</h2>
    By <AuthorLink :author="post.author" />
    <div>{{ getFormattedDate(post.publishDate) }}</div>
    <p class="post__description">{{ post.metaDescription }}</p>
    <article>
      {{ post.body }}
    </article>
    <ul>
      <li class="post__tags" v-for="tag in post.tags" :key="tag.name">
        <router-link :to="`/tag/${tag.name}`">#{{ tag.name }}</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
import gql from "graphql-tag";
import AuthorLink from "@/components/AuthorLink";
import { useQuery } from "@vue/apollo-composable";
import { useRoute } from "vue-router";
import { computed } from "vue";
import moment from "moment";

const POST_BY_SLUG = gql`
  query ($slug: String!) {
    postBySlug(slug: $slug) {
      title
      subtitle
      publishDate
      metaDescription
      slug
      body
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
  name: "Post",
  components: {
    AuthorLink,
  },
  methods: {
    getFormattedDate(date) {
      return moment(date).format("YYYY-MM-DD");
    },
  },
  setup() {
    const route = useRoute();
    const { result, loading, error } = useQuery(POST_BY_SLUG, {
      slug: route.params.slug,
    });
    const post = computed(() => result.value?.postBySlug ?? null);

    return {
      post,
      result,
      loading,
      error,
    };
  },
};
</script>