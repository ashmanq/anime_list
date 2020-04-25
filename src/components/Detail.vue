<template lang="html">
  <div class="">
    <h3>{{ anime.title }}</h3>
    <img v-bind:src="anime.image_url" alt="">
    <p> Airing Start: {{ anime.airing_start }}</p>
    <p> Episodes: {{ anime.episodes}} </p>
    <p> Score: {{ anime.score }}</p>

  </div>
</template>

<script>

// import {eventBus} from "../main.js";

export default {
  name: 'detail-view',
  props: ['anime'],

  methods: {
    // eventBus.$emit("selected-anime", anime);
  },
}

</script>

<style lang="css" scoped>
</style>
