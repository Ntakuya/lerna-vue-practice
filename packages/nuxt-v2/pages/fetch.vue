<template>
  <div>
    <div>{{ $fetchState.pending }} / {{ $fetchState.error }}</div>
    <p v-if="$fetchState.pending">
      <span class="loading"></span>
    </p>
    <p v-else-if="$fetchState.error">Error while fetching mountains 🤬</p>
    <ul v-else>
      <li v-for="mountain in mountains" :key="mountain.title">
        <NuxtLink
          :to="{ name: 'mountains-slug', params: { slug: mountain.slug } }"
        >
          {{ mountain.title }}
        </NuxtLink>
      </li>
    </ul>
    <button @click="$fetch">Refresh Data</button>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      mountains: [],
    }
  },
  async fetch() {
    this.mountains = await this.$axios.$get('https://api.nuxtjs.dev/mountains')
  },
  activated() {
    if (this.$fetchState.timestamp <= Date.now() - 30000) {
      this.$fetch()
    }
  },
}
</script>
