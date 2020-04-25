<template lang="html">
  <p v-on:click="itemDetail">{{ anime.title }}</p>
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
</style>
