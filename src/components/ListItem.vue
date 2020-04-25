<template lang="html">
  <div v-on:click="itemDetail" class="item-container">
    <img v-bind:src="anime.image_url" alt="">
    <h4>{{ aFunction() }}</h4>
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
    aFunction: function() {
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
  /* display:flex;
  flex-direction: column;
  width: 25%; */
  background-color: #f76363;
  border-radius: 5px;
  padding:20px;
}

.item-container:hover {
  background-color: #b34646;
}

img {
  max-height: 150px;
  max-width: 150px;
}
</style>
