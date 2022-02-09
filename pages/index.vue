<template>
  <main>
    <NuxtLink to="/">Home page</NuxtLink>
    <NuxtLink to="/about">
      About (internal link that belongs to the Nuxt App)
    </NuxtLink>
    <a href="https://nuxtjs.org">External Link to another page</a>
    <h1>I am rendered on the {{ renderedOn }} side</h1>
    <p v-if="$fetchState.pending">Fetching mountains...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>
    <div v-else>
      <h1>Nuxt Mountains</h1>
      <ul>
        <li v-for="mountain of mountains">{{ mountain.title }}</li>
      </ul>
      <button @click="$fetch">Refresh</button>
    </div>
    <div>
      <div>{{ content }}</div>
      <button @click="refresh">Refresh</button>
    </div>
  </main>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
  data() {
      return {
        mountains: []
      }
    },
  async fetch() {
    this.mountains = await fetch(
      'https://api.nuxtjs.dev/mountains'
    ).then(res => res.json())
  },
  activated() {
    // Call fetch again if last fetch more than 30 sec ago
    if (this.$fetchState.timestamp <= Date.now() - 30000) {
      this.$fetch()
    }
  },
  async asyncData({ params, $http }) {
  return {
      content: 'Created at: ' + new Date(),
      renderedOn: process.client ? 'client' : 'server',
    }
  },
  methods: {
    refresh() {
      this.$nuxt.refresh()
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start()
      setTimeout(() => this.$nuxt.$loading.finish(), 500)
    })
  }
})
</script>
