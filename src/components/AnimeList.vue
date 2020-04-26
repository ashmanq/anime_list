<template lang="html">
  <div class="">
    <h2>{{ title }}</h2>
    <div v-if="animes.length>0">
      <label for="genre">Genre</label>
      <select v-model="selectedGenre" v-on:change="resetPages" class="genre dropdown" name="genre" id="genre">
        <option value="">All</option>
        <option v-for="(genre, index) in findGenres" v-bind:value="genre">{{ genre }}</option>
      </select>
      <label for="no-per-page">No Per Page</label>
      <select v-model.number="noPerPage" class="dropdown" name="no-per-page" id="no-per-page">
        <option value=5>5</option>
        <option value=10 selected>10</option>
        <option value=15>15</option>
      </select>
      <div class="list-container">
        <list-item class="list-item noselect" v-for="(anime, index) in filteredList" :anime="anime" :key="index" :deletable="deletable"></list-item>
      </div>
      <p></p>
      <span v-on:click="changePage" id="pageDown" class="page-no noselect" v-if="pageNo>1"> {{ pageNo - 1 }} ⬅︎ </span>
      <span class=""> Page {{ pageNo }}</span>
      <span v-if="!endPage" v-on:click="changePage" id="pageUp" class="page-no noselect">  ➡︎ {{ pageNo +1 }}</span>
    </div>
    <div v-if="animes.length==0" class="empty-message">
      <p>You currently don't have anything in your watch list.</p>
      <p>Click on the Add to Watchlist button from the Anime Details page
      to add to youy list.</p>
    </div>
  </div>
</template>

<script>

import ListItem from './ListItem.vue';
import {eventBus} from '../main.js';

export default {
  name: "anime-list",
  props: ['animes', 'title', 'deletable'],
  data() {
    return {
        selectedGenre: "",
        listFiltered: this.filteredList,
        pageNo: 1,
        noPerPage: 10,
        endPage: false,
    }
  },

  methods: {

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
  margin-bottom:30px;
  margin-right: 10px;
}

.empty-message {
  margin-top: 60px;
}
.page-no{
  background-color: #2EC4B6;
  margin:20px;
  padding:10px;
  border-radius: 20px;
}

.page-no:hover{
  background-color: #b34646;
  cursor: pointer;
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

.dropdown:hover {
  cursor: pointer;
}

.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
     -khtml-user-select: none; /* Konqueror HTML */
       -moz-user-select: none; /* Old versions of Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
            user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Opera and Firefox */
}

@media only screen and (max-width: 900px) {
.list-item {
  width: 120px;
  margin:3px;
}

.page-no{
  margin:10px;
  padding:5px;
}
}

</style>
