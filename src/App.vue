<template>
  <div id="app">
    <img class="logo" src="../public/animelogo.svg" alt="">
    <h1>Anime Web App</h1>

    <div class="container">
      <section id="selection" class="selection">
        <anime-form></anime-form>
        <loading-indicator v-if="loading"></loading-indicator>
        <anime-list v-if="animes" :animes="animes" :title="listTitles[0]"></anime-list>
      </section>
      <a id="scrollTo">
        <section v-if="selectedAnime" id="details" class="details">
          <item-detail  :anime="selectedAnime"></item-detail>
        </section>
      </a>
      <!-- <section v-if="animeWatchList.length>0" class="watch-list"> -->
      <section class="watch-list">
        <anime-list :animes="animeWatchList" :title="listTitles[1]" :deletable="true"></anime-list>
        <genre-graph :data="animeWatchList"></genre-graph>
      </section>

    </div>
  </div>
</template>

<script>

import AnimeForm from './components/AnimeForm.vue';
import AnimeList from './components/AnimeList.vue';
import LoadingIndicator from './components/LoadingIndicator.vue';
import Detail from './components/Detail.vue';
import GenreGraph from './components/GenreGraph.vue';
import {eventBus} from './main.js';

export default {
  name: 'App',
  data() {
    return {
      animes: null,
      loading: null,
      selectedAnime: null,
      animeWatchList: [],
      listTitles: ['Currently Airing Anime', 'Watchlist']
    }
  },
  methods: {

  },
  computed: {
  },
  mounted() {
    this.loading = true;
    fetch("https://api.jikan.moe/v4/seasons/now")
    .then(res => res.json())
    .then((res) => {
      this.animes = res.data;
      this.loading = false;
    });

    // For when an item is selcted in the main anime list
    eventBus.$on("item-selected", (selected) => {
      this.selectedAnime = selected;
      var element = document.getElementById("scrollTo");
      setTimeout(()=>{element.scrollIntoView()}, 100);
    });
    // For when the genre filter is changed
    eventBus.$on("filter-genre", (selectedGenre) => {
      this.filterByGenre = selectedGenre;
    });
    // For when an anime is to be added to the favourites list
    eventBus.$on("add-to-watchlist", (animeToAdd) => {
      // const alreadyInWatchlist = this.animeWatchList.find(anime => anime.title === animeToAdd.title)
      const alreadyInWatchlist = this.animeWatchList.includes(animeToAdd);
      if(!alreadyInWatchlist) {
        this.animeWatchList.push(animeToAdd);
      }
    });
    // For removing an item from the watchlist
    eventBus.$on("delete-from-watchlist", (animeToRemove) => {
      const indexOfItemToRemove = this.animeWatchList.indexOf(animeToRemove);
      this.animeWatchList.splice(indexOfItemToRemove, 1);
    });

  },
  components: {
    "anime-form": AnimeForm,
    "anime-list": AnimeList,
    "item-detail": Detail,
    "loading-indicator": LoadingIndicator,
    "genre-graph": GenreGraph,
  }

}
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #4b4c57; */
  color:#ffffff;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-items: center;
}
h1  {
  font-size: 3em;
  margin-top:0px;
  margin-bottom:30px;
  color:#212f3d;
}
span{
  color:black;
}

.container {
  display: flex;
  justify-content: space-between;
}

.selection {
  background-color: #FFBF69;
  margin: 10px;
  padding:20px;
  border-radius: 5px;
  flex-grow:1;
}

.details {
  background-color: #FFBF69;
  margin:10px;
  padding:20px;
  border-radius: 5px;
  flex-grow:1;
}

.watch-list {
  background-color: #FFBF69;
  margin:10px;
  padding:20px;
  border-radius: 5px;
  flex-grow:1;
}
.logo {
  height:200px;
  width:200px;
  justify-content: center;
}
@media only screen and (max-width: 900px) {
  .container {
    flex-wrap: wrap;
    text-align: center;
  }
}
</style>
