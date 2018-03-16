<template>
  <v-app dark>
    <v-container class="mt-5">
      <v-layout wrap>
        <v-flex xs12 md6>
          <h2 class="mb-4">List of Postcodes searched</h2>
          <v-data-table
            :headers="headers"
            :items="postcodeArr"
            hide-actions
          >
            <template slot="items" slot-scope="props">
              <td class="text-xs-center">{{ props.item.postcode }}</td>
              <td class="text-xs-center">{{ props.item.suburb }}</td>
            </template>
            <template slot="no-data">
              <p>Fetching Data ...</p>
            </template>
          </v-data-table>
        </v-flex>
        <v-flex xs12 md5 offset-md1>
          <v-btn block class="success" @click="showGraph=true">Show Graph</v-btn>
          <line-chart v-if="showGraph" :data="chartData" :options="options"></line-chart>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";
import moment from "moment";
import LineChart from "./chart.vue";
export default {
  data() {
    return {
      showGraph: false,
      chartData: {
        labels: ["Morning", "Afternoon", "Evening", "Night"],
        datasets: [
          {
            label: "Most popular times of the day for searching",
            backgroundColor: ["#61c3ff", "#e31c79", "#eec414", "#9fe1b1"],
            data: [],
            borderColor: "white",
            borderWidth: 2
          }
        ]
      },
      options: {
        title: {
          fontColor: "white",
          display: true,
          text: "Most popular times of the day for searching"
        },
        legend: {
          labels: { fontColor: "white" }
        }
      },
      headers: [
        {
          text: "Postcode",
          sortable: false,
          value: "postcode",
          align: "center"
        },
        {
          text: "Suburb Name",
          sortable: false,
          value: "suburb",
          align: "center"
        }
      ],
      postcodeArr: [],
      timesArr: [{ morning: 0 }, { afternoon: 0 }, { evening: 0 }, { night: 0 }]
    };
  },
  components: {
    "line-chart": LineChart
  },
  beforeCreate() {
    let vm = this;
    let url = "https://gentle-reef-44595.herokuapp.com/";
    axios
      .get(url)
      .then(res => {
        vm.postcodeArr = res.data;
        vm.postcodeArr.forEach(e => {
          let createdHour = parseInt(moment(e.created_at).format("HH"));
          if (createdHour > 0 && createdHour < 10) {
            vm.timesArr[0].morning += 1;
          } else if (createdHour > 10 && createdHour < 15) {
            vm.timesArr[1].afternoon += 1;
          } else if (createdHour > 15 && createdHour < 19) {
            vm.timesArr[2].evening += 1;
          } else if (createdHour > 19 && createdHour < 24) {
            vm.timesArr[3].night += 1;
          }
        });
        vm.chartData.datasets[0].data.push(
          vm.timesArr[0].morning,
          vm.timesArr[1].afternoon,
          vm.timesArr[2].evening,
          vm.timesArr[3].night
        );
      })
      .catch(err => console.log(err));
  }
};
</script>

