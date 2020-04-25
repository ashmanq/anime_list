<template lang="html">
  <div class="">
    <h3>{{ anime.title }}</h3>
    <img v-bind:src="anime.image_url" alt="">
    <p> Airing Start: {{ formatDate }}</p>
    <p v-if="anime.episodes"> Episodes: {{ anime.episodes }} </p>
    <p> Type: {{ anime.type }}</p>
    <p> Source: {{ anime.source }}</p>
    <!-- <p> Score: {{ anime.score }}</p> -->
    <p><graph class="center" :score="anime.score"></graph></p>
  </div>
</template>

<script>

// import {eventBus} from "../main.js";
import Graph from './Graph.vue';

export default {
  name: 'detail-view',
  props: ['anime'],

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
  .center {
    margin: auto;
    width:120px;;
    align-self: center;
  }
</style>
