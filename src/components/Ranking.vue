<template>
  <div class="hello">
    <h1>{{ titleOption }}</h1>
    <select name="options" @change="rankingChange($event)">
      <option value="dailyStep">Daily Step</option>
      <option value="monthlyStep">Monthly Step</option>
    </select>

    <b-table-simple hover small caption-top responsive>
      <b-tr>
        <b-th colspan="1"></b-th>
        <b-td v-for="daily in dailyFields" :key="daily.id">
          <img :src="`https://picsum.photos/id/${daily.id - 50}/100/100`" />
        </b-td>
      </b-tr>
      <b-tr>
        <b-th>Name</b-th
        ><b-td v-for="daily in dailyFields" :key="daily.id">
          {{ daily.username }}
        </b-td>
      </b-tr>
      <b-tr>
        <b-th>Daily Step</b-th
        ><b-td v-for="daily in dailyFields" :key="daily.id">
          {{ daily.avg_steps }}
        </b-td>
      </b-tr>
      <b-tr>
        <b-th>Weekly Step</b-th>
        <b-td v-for="weekly in compFields" :key="weekly.id">
          {{ weekly.avg_steps }}
        </b-td>
      </b-tr>
      <b-tr>
        <b-th>Monthly Step</b-th>
        <b-td v-for="monthly in moCompFields" :key="monthly.id">
          {{ monthly.avg_steps }}
        </b-td>
      </b-tr>
    </b-table-simple>

    <!-- <b-card
      v-for="daily in dailyFields"
      :key="daily.id"
      :img-src="`https://picsum.photos/id/${daily.id - 50}/200/300`"
      img-alt="Image"
      tag="article"
      style="max-width: 13rem; display: flex; float: left"
    >
      <b>Name:</b> {{ daily.username }}<br />
      <b>Daily Steps:</b> {{ daily.avg_steps }}<br />
      <b>Last Week:</b> {{ parseInt(daily.avg_steps) }}<br />
      <b>Last Month:</b> {{ parseInt(daily.avg_steps) }}
    </b-card> -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      titleOption: "Sorted from highest to lowest daily",
      stepsD: "",
      dailyFields: [],
      weeklyFields: [],
      monthlyFields: [],
      compFields: [],
      moCompFields: [],
    };
  }, //?workouts_from=2021-02-01&workouts_to=2021-03-01
  async mounted() {
    await axios
      .get("https://step-meter-pp4publmdq-ez.a.run.app/users/", {
        headers: {
          Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
        },
      })
      .then((response) => {
        this.dailyFields = response.data.results.sort((a, b) => {
          if (a.avg_steps > b.avg_steps) {
            return -1;
          } else if (a.avg_steps < b.avg_steps) {
            return 1;
          } else {
            return 0;
          }
        });
      })
      .catch((error) => {
        console.log(error);
      });
    await axios
      .get(
        "https://step-meter-pp4publmdq-ez.a.run.app/users/?workouts_from=2021-02-14&workouts_to=2021-02-20",
        {
          headers: {
            Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
          },
        }
      )
      .then((response) => {
        this.weeklyFields = response.data.results;
        if (this.weeklyFields) {
          for (var x = 0; x < this.dailyFields.length; x++) {
            for (var y = 0; y < this.weeklyFields.length; y++) {
              if (this.dailyFields[x].id == this.weeklyFields[y].id) {
                this.compFields.push(this.weeklyFields[y]);
                break;
              }
            }
          }
        }
      })
      .catch((error) => {
        console.log(error);
      });
    await axios
      .get(
        "https://step-meter-pp4publmdq-ez.a.run.app/users/?workouts_from=2021-02-01&workouts_to=2021-03-01",
        {
          headers: {
            Authorization: "Token af61aed7399dc6a636e443cdc8a2d55db97c524a",
          },
        }
      )
      .then((response) => {
        this.monthlyFields = response.data.results;
        if (this.monthlyFields) {
          for (var x = 0; x < this.dailyFields.length; x++) {
            for (var y = 0; y < this.monthlyFields.length; y++) {
              if (this.dailyFields[x].id == this.monthlyFields[y].id) {
                this.moCompFields.push(this.monthlyFields[y]);
                break;
              }
            }
          }
        }
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
