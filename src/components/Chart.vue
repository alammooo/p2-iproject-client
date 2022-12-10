<script>
import { Bar } from "vue-chartjs"
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
  ArcElement,
} from "chart.js"
import axios from "axios"
import { mapActions, mapWritableState } from "pinia"
import { useCounterStore } from "../stores/counter"
import PictureLoader from "./PictureLoader.vue"

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
  ArcElement
)

export default {
  name: "BarChart",
  components: { Bar, PictureLoader },
  data() {
    return {
      loaded: false,
      covidData: [],
      diedCase: [],
      chartData: {
        labels: [],
        datasets: [
          {
            label: "Total Case",
            backgroundColor: [],
            data: [],
          },
        ],
      },
      chartOptions: {
        responsive: true,
      },
    }
  },
  methods: {
    ...mapActions(useCounterStore, ["openToast"]),

    async fetchCovidData() {
      try {
        this.loadPict = true
        const { data } = await axios({
          method: "GET",
          url: `${this.baseUrl}/covid-data`,
          headers: { access_token: localStorage.access_token },
        })
        // this.chartData.labels = [data.list_data[0].key]
        // this.chartData.datasets[0].data = [data.list_data[0].jumlah_kasus]

        const colorList = ["#6D67E4", "#46C2CB"]
        this.chartData.datasets[0].backgroundColor = data.list_data.map((el) => {
          const randomNumber = Math.floor(Math.random() * colorList.length)
          return colorList[randomNumber]
        })
        this.chartData.labels = data.list_data.map((el) => {
          return el.key
        })
        this.chartData.datasets[0].data = data.list_data.map((el) => {
          return el.jumlah_kasus
        })
        // console.log(this.covidData)
        this.loaded = true
      } catch (error) {
        this.openToast(error.response.data.message)
      } finally {
        this.loadPict = false
      }
    },
  },
  created() {
    this.fetchCovidData()
  },
  computed: {
    ...mapWritableState(useCounterStore, ["baseUrl", "loadPict"]),
  },
}
</script>

<template>
  <div class="w-3/4 h-1/2 mx-auto" v-if="!loadPict">
    <Bar
      v-if="loaded"
      id="my-chart-id"
      :options="chartOptions"
      :data="chartData"
      class="mt-24" />
  </div>
  <div class="flex flex-col items-center my-5" v-else>
    <PictureLoader />
  </div>
</template>
