<template>
  <div class="d-flex justify-center">
    <!-- <pre>{{ dates[index] }}</pre> -->
    <v-card class="mt-16" max-width="600">
      <v-card-title>
        <p>
          <span class="text-md-subtitle-1 text-body-2 font-weight-light"
            >Next Holiday</span
          >
          <br />
          <span class="text-md-h3 text-h5">{{ day }} </span>
        </p>
      </v-card-title>
      <v-card-subtitle
        ><span class="text-subtitle-1 font-weight-bold"
          >{{ name }}
        </span></v-card-subtitle
      >
    </v-card>
  </div>
</template>

<script>
import dayjs from "dayjs";
import Holidays from "../assets/holidays2021.json";

export default {
  data() {
    return {
      dates: [],
      index: 0,

      holidays: Holidays
    };
  },
  beforeMount() {
    this.dates = this.parseHolidays();
    this.index = this.nextHoliday();
  },
  methods: {
    nextHoliday() {
      for (const [index, date] of this.dates.entries()) {
        if (date.weight > 0) {
          return index;
        }
      }
    },
    parseHolidays() {
      const {
        response: { holidays }
      } = this.holidays;
      let dates = [];
      for (const holiday of holidays) {
        let { name } = holiday;
        let {
          date: { iso }
        } = holiday;
        let date = {
          name: name,
          date: dayjs(iso),
          weight: Math.ceil(dayjs(iso).diff(dayjs(), "d", true))
        };
        dates.push(date);
      }
      return dates;
    },
    getDayDisplay() {
      let weekday = this.dates[this.index].date.format("dddd");
      let day_of_month = this.dates[this.index].date.format("D");
      let month = this.dates[this.index].date.format("MMMM");
      let year = this.dates[this.index].date.format("YYYY");
      console.log(day_of_month[1]); // Array position 1 stores the lowest significant digit of the day of the month
      let order =
        day_of_month[1] == 1
          ? "st"
          : day_of_month[1] == 2
          ? "nd"
          : day_of_month[1] == 3
          ? "rd"
          : "th";
      return `${weekday} ${day_of_month}${order} ${month} ${year}`;
    },
    getNextHolidayName() {
      return this.dates[this.index].name;
    }
  },

  computed: {
    day: function() {
      return this.getDayDisplay();
    },
    name: function() {
      return this.getNextHolidayName();
    }
  }
};
</script>

<style></style>
