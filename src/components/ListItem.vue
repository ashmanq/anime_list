<template lang="html">
  <div class="container">
    <div v-on:click="itemDetail" class="item-container">
      <img v-bind:src="anime.image_url" alt="">
      <h4>{{ limitTitleLength() }}</h4>
        <!-- <button class="btn" v-if="deletable" v-on:click="deleteFromWatchList" type="button" name="button"><b>Remove</b></button> -->
    </div>
    <button class="btn" v-if="deletable" v-on:click="deleteFromWatchList" type="button" name="button"><b>Remove</b></button>
  </div>
</template>

<script>

import {eventBus} from '../main.js'
export default {
  name:'list-item',
  props: ['anime', 'deletable'],

  methods: {
    itemDetail: function() {
      eventBus.$emit('item-selected', this.anime);
    },
    deleteFromWatchList: function() {
      eventBus.$emit('delete-from-watchlist', this.anime);
    },
    limitTitleLength: function() {
      if (this.anime.title.length > 30) {
        return this.anime.title.substring(0, 30) + '...';
      } else {
        return this.anime.title;
      }
    },
  }
}
</script>

<style lang="css" scoped>
.container {
  display: flex;
  flex-direction: column;
  background-color: #2EC4B6;
  border-radius: 5px;
}
.item-container {
  height: 100%;
  padding: 20px 10px 0px 10px;
  background-color: #2EC4B6;
  border-radius: 5px;
  padding-top:20px;

  color: #f0ebeb;
}

.item-container:hover {
  background-color: #176F5E;
  color: #ffffff;
  cursor: pointer;
}

img {
  max-height: 150px;
  max-width: 110px;
  border-radius: 10px;
}
.btn{
  padding:10px;
  margin:10px;
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
  img {
    max-height: 130px;
    max-width: 100px;
  }
.container {
  flex-direction: row;
  padding-bottom: 100px;
}

.btn{
  margin:5px;
}

}
</style>
