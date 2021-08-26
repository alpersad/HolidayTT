<template>
  <div>
    <pre>{{ dates }}</pre>
  </div>
</template>

<script>
import dayjs from "dayjs";
import Holidays from "../assets/holidays2021.json";

export default {
  data() {
    return {
      dates: [],
      dates_index: 0,
      holidays: Holidays,
    };
  },
  mounted() {
    this.dates = this.parseHolidays();
    this.dates_index = this.nextHoliday();
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
        response: { holidays },
      } = this.holidays;
      let dates = [];
      for (const holiday of holidays) {
        let { name } = holiday;
        let {
          date: { iso },
        } = holiday;
        let date = {
          name: name,
          date: dayjs(iso),
          weight: dayjs(iso).diff(dayjs(), "d"),
        };
        dates.push(date);
      }
      return dates;
    },
  },
};
</script>

<style></style>
