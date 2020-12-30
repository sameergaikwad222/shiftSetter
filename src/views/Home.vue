<template>
  <div class="home">
    <div class="d-flex justify-start flex-wrap">
      <div v-for="(day, i) in getArrayDates" :key="i">
        <shiftDay
          :day="day.format('DD-MMM-YY')"
          :shiftPersons="shiftPersons(day)"
        />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import _ from "lodash";
import dayjs from "dayjs";
import employee from "@/data/employee";
import shiftDay from "@/components/shiftDay.vue";
export default {
  name: "Home",
  components: {
    shiftDay
  },
  data() {
    return {
      employee,
      fromDay: dayjs().format("YYYY-MM-DD"),
      toDay: dayjs()
        .add(7, "day")
        .format("YYYY-MM-DD")
    };
  },
  computed: {
    totalDays() {
      const date1 = dayjs(this.fromDay);
      const date2 = dayjs(this.toDay);
      let df1 = date2.diff(date1, "day");
      return df1;
    },
    getArrayDates() {
      var arr = [];
      var count = 0;
      var startDay = dayjs(this.fromDay);
      while (count < this.totalDays) {
        arr.push(startDay);
        startDay = startDay.add(1, "day");
        count++;
      }
      return arr;
    }
  },
  methods: {
    pickRandomResource(arrayResource, nResource) {
      arrayResource = arrayResource || [];
      nResource = nResource || 0;
      if (
        !nResource ||
        !arrayResource.length ||
        arrayResource.length < nResource
      )
        return null;
      else {
        const resourcelength = arrayResource.length;
        let randomArr = [];
        while (randomArr.length < nResource) {
          let randomResource = arrayResource[_.random(resourcelength - 1)];
          if (!randomArr.includes(randomResource))
            randomArr.push(randomResource);
        }
        return randomArr;
      }
    },
    shiftPersons(
      day,
      shiftResources = { morning: null, afternoon: null, night: null } //set null Three Resources if Not given explicitly
    ) {
      const resource = this.pickRandomResource(this.shiftSchedules(day), 3);
      shiftResources.morning = resource[0];
      shiftResources.afternoon = resource[1];
      shiftResources.night = resource[2];

      return shiftResources;
    },
    shiftSchedules(day) {
      //Returns Array of Available Resources
      console.log(`shiftschedule for ${day}`);
      // Code to be defined for Available resources.
      return this.employee;
    }
  }
};
</script>

<style scoped></style>
