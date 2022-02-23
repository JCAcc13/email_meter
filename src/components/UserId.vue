<template>
  <div id="container">
    <h1>{{ bName }}</h1>
    <input
      type="text"
      name="name"
      id="name"
      v-model="name"
      @keyup.enter="save"
    />
    <b-button v-on:click="save" style="margin-left: 5px">Enviar</b-button>
    <apexcharts
      class="graphic"
      width="500"
      type="bar"
      :options="options"
      :series="series"
    ></apexcharts>
    <apexcharts
      class="graphic"
      width="500"
      type="heatmap"
      :options="options"
      :series="series"
    ></apexcharts>
  </div>
</template>
<script>
import axios from "axios";
import VueApexCharts from "vue-apexcharts";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  components: {
    apexcharts: VueApexCharts,
  },

  data() {
    return {
      name: "christina73",
      bName: "",
      fields: [],
      dt: [],
      steps: [],
      options: {
        chart: {
          id: "apexchart-example",
        },
        xaxis: {
          categories: [],
        },
        fill: {
          color: ["#F44336", "#E91E63", "#9C27B0"],
        },
        plotOptions: {
          bar: {
            horizontal: false,
            dataLabels: {
              position: "bottom",
            },
          },
        },
        // grid: {
        //   row: {
        //     colors: ["#F44336", "#E91E63", "#9C27B0"],
        //   },
        //   column: {
        //     colors: ["#F44336", "#E91E63", "#9C27B0"],
        //   },
        // },
        legend: {
          labels: {
            colors: "#E91E63",
            useSeriesColors: false,
          },
          markers: {
            width: 12,
            height: 12,
            strokeWidth: 0,
            strokeColor: "#fff",
            fillColors: undefined,
            radius: 12,
            customHTML: undefined,
            onClick: undefined,
            offsetX: 0,
            offsetY: 0,
          },
        },
      },
      series: [
        {
          name: "Team A",
          data: [30, 40, 50, 60, 70, 80, 90],
        },
        {
          name: "Team B",
          data: [30, 40, 50, 60, 70, 80, 90],
        },
      ],
    };
  },
  async mounted() {
    await axios
      .get("https://step-meter-pp4publmdq-ez.a.run.app/christina73/workouts", {
        headers: {
          Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
        },
      })
      .then((response) => {
        this.fields = response.data.results;
        for (var i = 0; i < this.fields.length; i++) {
          this.dt.push(this.fields[i].date);
          this.steps.push(this.fields[i].steps);
        }
        this.options = {
          chart: {
            id: "apexchart-example",
          },
          xaxis: {
            categories: this.dt,
          },
        };
        this.series = [
          {
            name: "Steps",
            data: this.steps,
          },
        ];
      });
  },
  methods: {
    save: function () {
      this.bName = this.name;
      axios
        .get(
          `https://step-meter-pp4publmdq-ez.a.run.app/${this.bName}/workouts`,
          {
            headers: {
              Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
            },
          }
        )
        .then((response) => {
          this.fields = response.data.results;
          if (response.data.results != "") {
            this.dt = [];
            this.steps = [];
            for (var i = 0; i < this.fields.length; i++) {
              this.dt.push(this.fields[i].date);
              this.steps.push(this.fields[i].steps);
            }
            this.options = {
              chart: {
                id: "apexchart-example",
              },
              xaxis: {
                categories: this.dt,
              },
            };
            this.series = [
              {
                name: "Steps",
                data: this.steps,
              },
            ];
          }
        });
    },
  },
};
</script>
<style scoped lang="scss">
.graphic {
  margin-left: 26%;
}
</style>
