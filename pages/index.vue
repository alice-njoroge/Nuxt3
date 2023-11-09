<script lang="ts" setup>
import {Bar} from "vue-chartjs";
import type {ChartData} from "chart.js";

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import {fetch} from "ofetch";

ChartJS.register(
    Title,
    Tooltip,
    Legend,
    BarElement,
    CategoryScale,
    LinearScale)

//fetching data from the fake API
type movie = { title: string, rating: number };
const movies = ref<movie[]>([]);
const chartOptions = {
  responsive: true,
  scales: {
    x: {
      grid: {
        display: false // don't display the x-axis grid lines
      }
    },
    y: {
      ticks: {
        stepSize: 1000 // display data on y-axis at intervals of 100
      }
    }
  }
}
fetch("/api.json")
    .then(async (res) => movies.value = await res.json())
    .catch(err => console.log(err.message))

const chartData = computed(() : ChartData<"bar"> =>{
  return {
    labels: movies.value.map(movie => movie.title),
    datasets: [
      {
        label: "Prix RBEU",
        backgroundColor: "#5B9CD5",
        data: movies.value.map(movie => movie.blue)
      },
      {
        label: "Prix Collecte",
        backgroundColor: "#ee7d30",
        data: movies.value.map(movie => movie.orange)
      }
    ],

  }

})

</script>

<template>
  <Bar v-if="movies.length"
      :data="chartData"
      :options="chartOptions"
     />
</template>

<style scoped>
body {
  padding: 100px;
}
</style>
