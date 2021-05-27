<template>
  <div class="p-10">
    <div class="flex items-center justify-between">
      <div class="text-4xl">
        {{ currentMonthYear.format("MMMM YYYY") }}
      </div>

      <div class="flex gap-4">
        <button @click="prev" class="px-4 py-2 bg-gray-100 rounded hover:bg-green-500 hover:text-white">&lt; Prev</button>
        <button @click="today" class="px-4 py-2 bg-gray-100 rounded hover:bg-green-500 hover:text-white">Today</button>
        <button @click="next" class="px-4 py-2 bg-gray-100 rounded hover:bg-green-500 hover:text-white">Next &gt;</button>
      </div>
    </div>

    <hr class="my-10" />

    <div class="grid grid-cols-7">
      <div v-for="day in daysInWeek" :key="day">{{ day }}</div>
      <div v-for="(day, index) in calendars" :key="index" class="h-32 m-0.5 rounded-lg text-xl bg-gray-100">
        <div class="p-2">
          <span v-if="day.isToday" v-html="day.date.format('D')" class="px-2 text-white bg-green-400 rounded-full"></span>
          <span v-if="!day.isToday && day.date" v-html="day.date.format('D')" class="text-black" ></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import dayjs from "dayjs";
import 'dayjs/locale/id';
dayjs.locale('id');

export default defineComponent({
  data() {
    return {
      currentMonthYear: dayjs(),
      calendars: [],
    };
  },

  computed: {
    daysInWeek: () => [0, 1, 2, 3, 4, 5, 6].map((i) => dayjs().day(i).format('dddd'))
  },

  mounted() {
    this.currentMonthYear = dayjs();
    this.generateCalendar();
  },

  methods: {
    generateCalendar() {
      this.calendars = [];

      const firstDayIndex = this.currentMonthYear.date(1).day();
      const lastDayInMonth = this.currentMonthYear.daysInMonth();

      for (let i = 0; i < firstDayIndex; i++) {
        this.calendars.push({ isToday: false, date: null });
      }

      for (let i = 1; i <= lastDayInMonth; i++) {
        const day = {
          isToday: this.currentMonthYear.date(i).toString() === dayjs().toString(),
          date: this.currentMonthYear.date(i),
        };
        this.calendars.push(day);
      }
    },

    prev() {
      this.currentMonthYear = this.currentMonthYear.subtract(1, "month");
      this.generateCalendar();
    },

    next() {
      this.currentMonthYear = this.currentMonthYear.add(1, "month");
      this.generateCalendar();
    },

    today() {
      this.currentMonthYear = dayjs();
      this.generateCalendar();
    },
  },
});
</script>
