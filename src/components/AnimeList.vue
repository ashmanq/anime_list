<template lang="html">
  <div class="">
    <label for="genre">Genre </label>
    <select v-model="selectedGenre" v-on:change="resetPages" class="genre" name="genre">
      <option value="">Select Genre</option>
      <option v-for="(genre, index) in findGenres" v-bind:value="genre">{{ genre }}</option>
    </select>
    <div class="list-container">
      <list-item class="list-item" v-for="(anime, index) in filteredList" :anime="anime" :key="index"></list-item>
    </div>
    <p></p>
    <span v-on:click="changePage" id="pageDown" class="page-no" v-if="pageNo>1"> {{ pageNo - 1 }} ⬅︎ </span>
    <span class=""> Page {{ pageNo }}</span>
    <span v-if="!endPage" v-on:click="changePage" id="pageUp" class="page-no">  ➡︎ {{ pageNo +1 }}</span>
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
        listFiltered: this.filteredList,
        pageNo: 1,
        noPerPage: 12,
        endPage: false,
    }
  },

  methods: {
    // filterGenre: function() {
    //   // eventBus.$emit('filter-genre', this.selectedGenre);
    // },
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
      return genres;
    },

    filteredList: function() {
      let animeToDisplay = null;
      this.endPage = false;
      if(this.selectedGenre){
        animeToDisplay = this.animes.filter((anime) => {
          return anime.genres.some(genre => genre.name === this.selectedGenre);
        });
      } else {
        animeToDisplay = this.animes;
      }
      // We then pagenate the result
      const startSlice = ((this.pageNo - 1) * this.noPerPage);
      let endSlice = ((this.pageNo * this.noPerPage));
      if (endSlice >= animeToDisplay.length) {
        endSlice = animeToDisplay.length ;
        this.endPage = true;
      }
      return animeToDisplay.slice(startSlice, endSlice);

    } ,
  },
  methods: {
    changePage: function(e){
      if(e.target.id === "pageUp") {
        this.pageNo += 1;
      }
      if(e.target.id === "pageDown") {
        this.pageNo -= 1;
      }

    },

    resetPages: function() {
      this.pageNo = 1;
    },
  },
  components: {
    'list-item': ListItem,
  }
}
</script>

<style lang="css" scoped>
select {
  margin:30px;
}
.page-no{
  background-color: #f76363;
  margin:20px;
  padding:10px;
}

.page-no:hover{
  background-color: #b34646;
}
.list-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.list-item{
  width: 160px;
  margin:5px;
}
</style>
