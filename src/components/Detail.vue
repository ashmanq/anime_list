<template lang="html">
  <div class="">
    <h3>{{ anime.title }}</h3>
    <img v-bind:src="anime.image_url" alt="">
    <p> Airing Start: {{ anime.airing_start }}</p>
    <p> Episodes: {{ anime.episodes}} </p>
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

  methods: {
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
