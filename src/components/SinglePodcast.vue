<template>
  <div v-if="data">
    <a
      class="podcast-link"
      :href="'https://www.acast.com/blindboy/' + data.url"
      target="_blank"
      rel="noopener noreferrer"
    >
      <h2 class="podcast-link-title">{{ data.name }}</h2>
      <div v-html="data.description" />
    </a>
  </div>
</template>

<script>
  export default {
    name: 'SinglePodcast',
    
    props: ['data']
  }
</script>

<style>
  .podcast-link {
    color: #0e0c11;
    text-decoration: none;
  }
  
  .podcast-link:hover .podcast-link-title {
    color: #f81a2f;
  }
  
  .podcast-link p > br {
    display: none;
  }
</style>