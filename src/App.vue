<template>
  <div id="app">
    <h1>Anime Web App</h1>
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
      // var element = document.getElementById("app");
      // element.scrollIntoView();
      this.selectedAnime = selected;
    });

    eventBus.$on("filter-genre", (selectedGenre) => {
      this.filterByGenre = selectedGenre;
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


</style>
