<template lang="html">
  <div class="">
    <div class="chart" v-if="watchListData.length>0">
      <GChart
      :settings="{ packages: ['corechart'] }"
      type="PieChart"
      :data="compInputs"
      :options="options"
      />
      <p> This is the weighted genre mix of the anime you have included in your watch list.</p>
      <p> Mouse over a slice to get more details.</p>
    </div>

  </div>
</template>

<script>
// import * as d3 from 'd3';
import {GChart} from 'vue-google-charts';

export default {
  name:'genre-chart',
  props:['data'],
  data() {
    return {
      watchListData: this.data,
      options: {
        title: "Genre Mix",
        titleTextStyle: {
          fontName: 'Roboto',
          color: "#ffffff",
          fontSize:25,
        },
        width:300,
        height:300,
        pieHole: 0.5,
        legend: 'none',
        backgroundColor: "#FFBF69",
      },
      chartInput: [
        ['Task', 'Hours per Day'],
       ['Work',     11],
       ['Eat',      2],
       ['Commute',  2],
       ['Watch TV', 2],
       ['Sleep',    7]
     ]
    }
  },
  mounted() {
    // console.log(this.watchListData);
  },
  methods: {

  },
  computed: {
    // Function to manipulate data into a form that can be used by the
    // GChart component to show as a pie chart.
    compInputs: function(){
      const result = [['Genre', 'Occurence']];
      const genres = [];

      // We then want to get the genres from each anime and give them a weighting.
      // For simplicity each genre for an individual anime will be wighted as
      // 1 divided by the total number of genres for that anime
      const genreDataList = this.watchListData.flatMap((anime) => {
        return anime.genres.map((genre) => {
          const weightedValue = 1/anime.genres.length;
          genres.push(genre.name);
          return { [genre.name]: weightedValue};
        });
      })

      // We get all the unique genre values from the watchlist
      const uniqueGenres = new Set(genres);

      // We sum all weighted genres together to get a list of all the summated
      // genres
      uniqueGenres.forEach((genre) => {
       const genreTotalWeighting = genreDataList.reduce((total, dataListGenre) => {
          if(dataListGenre[genre]) {
            total += dataListGenre[genre];
          };
          return total;
       }, 0);
       result.push([genre, genreTotalWeighting])
      });

      return result;
    },
  },
  components: {
    GChart,
  }
}
</script>

<style lang="css" scoped>
.chart {
  padding:1px;
  margin-top: 20px;
  /* background-color: #2EC4B6;
  border-radius: 2px; */
}
</style>
