<template>
  <router-link :to="`/author/${author.user.username}`">{{
    author.user.firstName
  }}</router-link>
</template>

<script>
export default {
  name: "AuthorLink",
  props: {
    author: {
      type: Object,
      required: true,
    },
  },
};
</script>