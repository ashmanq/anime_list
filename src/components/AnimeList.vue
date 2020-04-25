<template lang="html">
  <div class="list-container">
    <list-item class="list-item" v-for="(anime, index) in animes" :anime="anime" :key="index"></list-item>
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

.list-container {
  display: flex;
  flex-wrap: wrap;
}

.list-item{
  width: 160px;
  margin:5px;
}
</style>
