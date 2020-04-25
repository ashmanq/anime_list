<template lang="html">
  <div class="">
    <label for="genre">Genre</label>
    <select v-model="selectedGenre" v-on:change="filterGenre" class="genre" name="genre">
      <option value="">Select Genre</option>
      <option v-for="(genre, index) in findGenres" v-bind:value="genre">{{ genre }}</option>
    </select>
    <div class="list-container">
      <list-item class="list-item" v-for="(anime, index) in filteredList" :anime="anime" :key="index"></list-item>
    </div>
  </div>
</template>

<script>

import ListItem from './ListItem.vue';
import {eventBus} from '../main.js';

export default {
  name: "anime-list",
  props: ['animes'],
  data() {
    return {
        selectedGenre: "",
    }
  },

  methods: {
    filterGenre: function() {
      // eventBus.$emit('filter-genre', this.selectedGenre);
    },
  },
  computed: {
    // Function to get all genres from anime list and create
    // a list of all the unique genres to populate the filter
    // drop down list.
    findGenres: function() {
      const genresArrays = this.animes.map((anime) => {
        return anime.genres.flatMap(x => x.name);
      });
      const allGenres = genresArrays.flat();
      const genres = [...new Set(allGenres)];
      // genres.unshift('All');
      return genres;
    },

    filteredList: function() {
      if(this.selectedGenre){
        return this.animes.filter((anime) => {
          return anime.genres.some(genre => genre.name === this.selectedGenre);
        });
      } else {
        return this.animes;
      }
    } ,
  },
  components: {
    'list-item': ListItem,
  }
}
</script>

<style lang="css" scoped>

.list-container {
  display: flex;
  flex-wrap: wrap;
}

.list-item{
  width: 160px;
  margin:5px;
}
</style>
