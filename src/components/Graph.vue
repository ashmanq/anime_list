<template lang="html">
  <GChart
  :settings="{ packages: ['corechart', 'gauge'] }"
  type="Gauge"
  :data="chartInput"
  :options="chartOptions"
  />
</template>

<script>

import {GChart} from 'vue-google-charts';

export default {
  name: "anime-graph",
  props: ['score'],
  data() {
    return {
     chartOptions: {
       width: 120, height: 120,
      redFrom: 9, redTo: 10,
      yellowFrom:7, yellowTo: 9,
      max:10, minorTicks: 5
     }
    }
  },

  computed: {
    chartInput: function() {
      const data = [['Label', "Value"], ['Rating', this.score]];
      return data;
      console.log(data);
    }
  },
  components: {
    GChart,
  }
}
</script>

<style lang="css" scoped>
</style>
