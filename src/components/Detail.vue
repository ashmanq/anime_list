<template lang="html">
  <div class="">
    <h2>Anime Details</h2>
    <div class="details">
      <h3>{{ anime.title }}</h3>
      <div class="row">
        <div class="column">
          <img v-bind:src="anime?.images?.webp?.image_url" alt="">
          <p>{{ anime.synopsis }}</p>
        </div>
        <div class="column">
          <p> Airing Start: {{ formatDate }}</p>
          <p v-if="anime.episodes"> Episodes: {{ anime.episodes }} </p>
          <p> Type: {{ anime.type }}</p>
          <p> Source: {{ anime.source }}</p>
          <p v-for="(producer, index) in anime.producers" >Producer(s): {{ producer.name }}</p>
          <p><graph class="center" :score="anime.score"></graph></p>
          <button v-on:click="addToWatchlist" type="button" name="button" class="btn"><b>Add to Watchlist</b></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import {eventBus} from "../main.js";
import Graph from './Graph.vue';

export default {
  name: 'detail-view',
  props: ['anime'],
  methods: {
    addToWatchlist: function() {
      eventBus.$emit("add-to-watchlist", this.anime);
    },
  },
  computed: {
    formatDate: function() {
      const date = new Date(this.anime.airing_start);
      const formatedDate = `${date.getDate()}/${date.getMonth()}/${date.getFullYear()}`;
      return formatedDate;
    },
    // eventBus.$emit("selected-anime", anime);
  },
  components: {
    'graph': Graph,
  }
}

</script>

<style lang="css" scoped>

img {
  padding:16px;
  height:300px;
  max-width: 200px;
  align-self: center;
  border-radius: 30px;
}
.details {
  background-color: #2EC4B6;
  color: #f0ebeb;
  margin-top:100px;
}
.row {
  display:flex;
  justify-content: space-around;
}
.column {
  display: flex;
  flex-direction: column;
  margin:5px;
  max-width: 300px;
}
.center {
  margin: auto;
  width:120px;
  align-self: center;
}

.btn{
  padding:5px;
  border-radius: 80px;
  font-size: 1em;
  color: #636575;
  background-color: #FFBF69;
  border-style: none;
}
.btn:hover {
  background-color: #FF9F1C;
  cursor: pointer;
}

@media only screen and (max-width: 900px) {
  .row {
    flex-wrap: wrap;
  }
  .center {
    margin: auto;
    width:100px;
    align-self: center;
  }

  img {
    padding:16px;
    height:200px;
    max-width: 150px;
    align-self: center;

  }
}
</style>
