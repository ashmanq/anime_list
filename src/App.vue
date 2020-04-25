<template>
  <div id="app">
    <h1>Anime Currently Airing</h1>
    <section class="selection">
      <anime-list :animes="animes"></anime-list>
    </section>
    <section class="details">

    </section>
  </div>
</template>

<script>

import AnimeList from './components/AnimeList.vue';

export default {
  name: 'App',
  data() {
    return {
      animes: null,
    }
  },
  components: {
    "anime-list": AnimeList,
  },
  mounted() {
    fetch("https://api.jikan.moe/v3/season")
    .then(res => res.json())
    .then(res => this.animes = res.anime);
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
}
</style>
