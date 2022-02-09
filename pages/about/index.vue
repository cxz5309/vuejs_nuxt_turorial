<template>
  <main>
    <div>
      <h1>{{ post.title }}</h1>
      <p>{{ post.description }}</p>
    </div>
  </main>
</template>

<script lang="ts">
import Vue from 'vue'

export default {
  async asyncData({ params, $http }) {
  const post = await $http.$get(`https://api.nuxtjs.dev/posts/${params.id}`)

  return {
      post
    }
  },
}
</script>
