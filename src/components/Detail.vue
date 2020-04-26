<template lang="html">
  <div class="">
    <h2>Anime Details</h2>
    <div class="details">
      <h3>{{ anime.title }}</h3>
      <div class="row">
        <div class="column">
          <img v-bind:src="anime.image_url" alt="">
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

}
.details {
  background-color: #2EC4B6;
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
</style>
