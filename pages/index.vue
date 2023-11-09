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
fetch("/api.json")
    .then(async (res) => movies.value = await res.json())
    .catch(err => console.log(err.message))

const chartData = computed(() : ChartData<"bar"> =>{
  return {
    labels: movies.value.map(movie => movie.title),
    datasets: [
      {
        backgroundColor: ["#c82834", "#244771"],
        data: movies.value.map(movie => movie.rating)
      }
    ]

  }

})

</script>

<template>
  <Bar v-if="movies.length"
      :data="chartData"
      :options="{
        plugins:{
          legend:{display: false}
        }
      }"/>
</template>

<style scoped>
body {
  padding: 100px;
}
</style>
