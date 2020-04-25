<template>
  <div id="app">
    <h1>Anime Currently Airing</h1>
    <div class="container">
      <section class="selection">
        <loading-indicator v-if="loading"></loading-indicator>
        <anime-list v-if="animes" :animes="animes"></anime-list>
      </section>
      <section class="details">
        <item-detail v-if="selectedAnime" :anime="selectedAnime"></item-detail>
      </section>
    </div>
  </div>
</template>

<script>

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
    }
  },

  methods: {

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
      this.selectedAnime = selected;
    });
  },
  components: {
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
