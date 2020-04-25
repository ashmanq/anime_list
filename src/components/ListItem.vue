<template lang="html">
  <div v-on:click="itemDetail" class="item-container">
    <img v-bind:src="anime.image_url" alt="">
    <h3>{{ anime.title }}</h3>
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
  }
}
</script>

<style lang="css" scoped>
.item-container {
  /* display:flex;
  flex-direction: column;
  width: 25%; */
}

img {
  max-height: 200px;
  max-width: 150px;
}
</style>
