<template lang="html">
  <div class="details">
    <h2>Anime Details</h2>
    <h3>{{ anime.title }}</h3>

    <div class="row">

      <img v-bind:src="anime.image_url" alt="">
      <section class="column">
        <p> Airing Start: {{ formatDate }}</p>
        <p v-if="anime.episodes"> Episodes: {{ anime.episodes }} </p>
        <p> Type: {{ anime.type }}</p>
        <p> Source: {{ anime.source }}</p>
        <p v-for="(producer, index) in anime.producers" >Producer(s): {{ producer.name }}</p>
        <!-- <p> Score: {{ anime.score }}</p> -->
        <p><graph class="center" :score="anime.score"></graph></p>
        <button type="button" name="button">Add to Favourites</button>
      </section>
    </div>
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

img {
  padding:16px;
  height:300px;
  align-self: center;

}
.details {
  background-color: #f76363;
  margin-top:80px;
}
.row {
  display:flex;
  justify-content: space-around;
}
.column {
  display: flex;
  flex-direction: column;
}
.center {
  margin: auto;
  width:120px;
  align-self: center;
}
</style>
