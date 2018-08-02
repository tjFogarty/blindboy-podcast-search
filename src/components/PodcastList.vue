<template>
  <div>
    <h1>The Blindboy Podcast</h1>
    
    <div class="podcast-search">
      <label for="search" class="podcast-search-label">Search:</label>
      <input class="podcast-search-input" v-model="query" id="search" />
    </div>
    
    <ul v-if="filteredPodcasts.length" class="podcast-list">
      <li v-for="podcast in filteredPodcasts" :key="podcast.id">
        <SinglePodcast :data="podcast" />
      </li>
    </ul>
    
    <div v-else-if="podcasts.length && !filteredPodcasts.length">
      <h2>No results found for "{{ query }}"</h2>
    </div>
    
    <div v-else>
      <h2>Loading podcasts</h2>
    </div>
  </div>
</template>

<script>
import SinglePodcast from './SinglePodcast.vue'
import axios from 'axios'
import Fuse from 'fuse.js'

const endpoint = 'https://www.acast.com/api/channels/blindboy/acasts'

export default {
  name: 'PodcastList',
  
  components: { SinglePodcast },
  
  data() {
    return {
      podcasts: [],
      fuse: null,
      query: ''
    }
  },
  
  computed: {
    filteredPodcasts() {
      if (this.query) {
        return this.fuse.search(this.query)
      }
      
      return this.podcasts
    }
  },
  
  created() {
    this.getAllPodcasts()
  },
  
  methods: {
    async getAllPodcasts() {
      const [page1, page2, page3, page4, page5, page6] = await Promise.all([
        axios.get(`${endpoint}`),
        axios.get(`${endpoint}?page=1`),
        axios.get(`${endpoint}?page=2`),
        axios.get(`${endpoint}?page=3`),
        axios.get(`${endpoint}?page=4`),
        axios.get(`${endpoint}?page=5`)
      ]);
      
      this.podcasts = [
        ...page1.data,
        ...page2.data,
        ...page3.data,
        ...page4.data,
        ...page5.data,
        ...page6.data
      ]
      
      this.setupSearch()
    },
    
    setupSearch() {
      let options = {
        shouldSort: true,
        threshold: 0.5,
        location: 0,
        distance: 100,
        maxPatternLength: 32,
        minMatchCharLength: 1,
        keys: [
          'name',
          'subtitle'
        ]
      }
      
      this.fuse = new Fuse(this.podcasts, options)
    }
  }
}
</script>

<style>
  .podcast-list {
    display: grid;
    grid-template-columns: repeat(3, minmax(250px, auto));
    grid-gap: 20px;
    list-style: none;
    margin: 0;
    padding: 0;
  }
  
  .podcast-search {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .podcast-search-label {
    display: block;
    margin-right: 10px;
  }
  
  .podcast-search-input {
    flex-grow: 1;
  }
</style>
