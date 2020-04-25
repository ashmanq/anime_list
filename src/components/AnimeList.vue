<template lang="html">
  <div class="">
    <list-item v-for="(anime, index) in animes" :anime="anime" :key="index"></list-item>
  </div>
</template>

<script>

import ListItem from './ListItem.vue';

export default {
  name: "anime-list",
  props: ['animes'],

  components: {
    'list-item': ListItem,
  }
}
</script>

<style lang="css" scoped>
</style>
