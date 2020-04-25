<template lang="html">
  <div class="">
    <label for="genre">Genre</label>
    <select class="genre" name="genre">
      <option v-for="(genre, index) in findGenres" v-bind:value="genre">{{ genre }}</option>
    </select>
    <div class="list-container">
      <list-item class="list-item" v-for="(anime, index) in animes" :anime="anime" :key="index"></list-item>
    </div>
  </div>
</template>

<script>

import ListItem from './ListItem.vue';

export default {
  name: "anime-list",
  props: ['animes'],

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

      return genres;
    },
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
