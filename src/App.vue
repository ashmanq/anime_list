<template>
  <div id="app">
    <h1>Anime Currently Airing</h1>
    <div class="container">
      <section id="selection" class="selection">
        <anime-form></anime-form>
        <loading-indicator v-if="loading"></loading-indicator>
        <anime-list v-if="animes" :animes="animes"></anime-list>
      </section>
      <section id="details" class="details">
        <item-detail v-if="selectedAnime" :anime="selectedAnime"></item-detail>
      </section>
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
      // filterByGenre: null,
    }
  },
  methods: {

  },
  computed: {
    filteredList: function() {
      if(this.filterByGenre){
        return this.animes.filter((anime) => {
          anime.genres.some(genre => genre.name === this.filterByGenre);
        });
      } else {
        return this.animes;
      }

    } ,
  },
  mounted() {
    this.loading = true;
    fetch("https://api.jikan.moe/v3/season")
    .then(res => res.json())
    .then((res) => {
      this.animes = res.anime;
      this.loading = false;
    });

    eventBus.$on("item-selected", (selected) => {
      var element = document.getElementById("app");
      element.scrollIntoView();
      this.selectedAnime = selected;
    });

    eventBus.$on("filter-genre", (selectedGenre) => {
      this.filterByGenre = selectedGenre;
      console.log("hi");
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
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}
h1  {
  margin:60px;
}
.container {
  display: flex;
  justify-content: space-between;

}

.selection {
  flex:1;
}

.details {
  justify-content: center;
  align-items: center;
  flex:1;
}


</style>
