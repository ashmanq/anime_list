<template lang="html">
  <div v-on:click="itemDetail" class="item-container">
    <img v-bind:src="anime.image_url" alt="">
    <h4>{{ limitTitleLength() }}</h4>
  </div>
</template>

<script>

import {eventBus} from '../main.js'
export default {
  name:'list-item',
  props: ['anime'],

  methods: {
    itemDetail: function(){
      eventBus.$emit('item-selected', this.anime);
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
.item-container {
  background-color: #2EC4B6;
  border-radius: 5px;
  padding-top:20px;
  padding: 20px 10px 0px 10px;
  color: #f0ebeb;
}

.item-container:hover {
  background-color: #176F5E;
  color: #ffffff;
  cursor: pointer;
}

img {
  max-height: 150px;
  max-width: 150px;
}
</style>
