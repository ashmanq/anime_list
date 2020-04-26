<template>
  <div id="app">
    <h1>Anime Web App</h1>
    <div class="container">
      <section id="selection" class="selection">
        <anime-form></anime-form>
        <loading-indicator v-if="loading"></loading-indicator>
        <anime-list v-if="animes" :animes="animes" :title="listTitles[0]"></anime-list>
      </section>
      <div v-if="animeWatchList.length>0" class="watch-list">
        <anime-list :animes="animeWatchList" :title="listTitles[1]"></anime-list>
      </div>
      <div v-if="selectedAnime" id="details" class="details">
        <item-detail  :anime="selectedAnime"></item-detail>
      </div>

    </div>
  </div>
</template>

<script>

import AnimeForm from './components/AnimeForm.vue';
import AnimeList from './components/AnimeList.vue';
import LoadingIndicator from './components/LoadingIndicator.vue';
import Detail from './components/Detail.vue';
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
      // filterByGenre: null,
    }
  },
  methods: {

  },
  computed: {
  },
  mounted() {
    this.loading = true;
    fetch("https://api.jikan.moe/v3/season")
    .then(res => res.json())
    .then((res) => {
      this.animes = res.anime;
      this.loading = false;
    });
    // For when an item is selcted in the main anime list
    eventBus.$on("item-selected", (selected) => {
      // var element = document.getElementById("app");
      // element.scrollIntoView();
      this.selectedAnime = selected;
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


    })

  },
  components: {
    "anime-form": AnimeForm,
    "anime-list": AnimeList,
    "item-detail": Detail,
    "loading-indicator": LoadingIndicator,
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #e0f0ff;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}
h1  {
  margin:60px;
  color:#212f3d;
}
.container {
  display: flex;
  justify-content: space-between;
}

.selection {
  flex:2;
  background-color: #3d54ff;
  margin: 10px;
  padding:20px;
  border-radius: 5px;
}

.details {
  background-color: #3d54ff;
  margin:10px;
  padding:20px;
  border-radius: 5px;
  justify-content: center;
  align-items: center;
  flex:1;
}

.watch-list {
  background-color: #3d54ff;
  margin:10px;
  padding:20px;
  border-radius: 5px;
  flex:1;
}


</style>
