<template lang="">
  <div class="sweet-vue-calendar">
    <div class="sweet-header" :style="Object.keys(headerStyle).length > 0 ? headerStyle : {}">
      <slot
        name="header"
        :events="{
          next: () => next(),
          previous: () => previous(),
          current: () => current(),
        }"
        :date="dateFormat"
      >
        <slot name="header-date" :date="dateFormat">
          <p class="sweet-date">{{ dateFormat }}</p>
        </slot>
        <slot
          name="control-buttons"
          :events="{
            next: () => next(),
            previous: () => previous(),
            current: () => current(),
          }"
        >
          <div>
            <button class="sweet-btn" @click="previous">Anterior</button>
            <button class="sweet-btn" @click="current">Hoje</button>
            <button class="sweet-btn" @click="next">Proximo</button>
          </div>
        </slot>
      </slot>
    </div>
    <div class="sweet-days-month">
      <template v-for="(week, index) in config.weekdays" :key="index">
        <slot name="week" :week="longWeek ? week : config.weekdaysShort[index]">
          <div class="sweet-week">
            {{ (longWeek && !isMobile ) && (size === 'medium' || size === 'large') ? week : config.weekdaysShort[index] }}
          </div>
        </slot>
      </template>
      <template v-for="(day, index) in days" :key="index">
        <slot name="day" :day="day">
          <div
            :class="{ 'sweet-day': true, 'sweet-day-off': !day.currentMonth }"
          >
            {{ day.value }}
          </div>
        </slot>
      </template>
    </div>
  </div>
</template>
<script>
import moment from "moment";
export default {
  props: {
    config: {
      type: Object,
      default: {},
    },
    longWeek: {
      type: Boolean,
      default: false,
    },
    size:{
      type:String,
      default:''
    },
    headerStyle:{
      type:Object,
      default:{}
    }
  },
  data() {
    moment.defineLocale(this.config.locale, {
      months: this.config.months ? this.config.months : [],
      weekdays: this.config.weekdays ? this.config.weekdays : [],
      weekdaysShort: this.config.weekdaysShort ? this.config.weekdaysShort : [],
      monthsShort: this.config.monthsShort ? this.config.monthsShort : [],
    });
    return {
      date: moment().valueOf(),
    };
  },
  computed: {
    day() {
      return moment(this.date).format("DD");
    },
    month() {
      return moment(this.date).format("MM");
    },
    year() {
      return moment(this.date).format("YYYY");
    },
    days() {
      return this.getAllDaysOfMonth();
    },
    isMobile() {
      let width =
        window.innerWidth ||
        document.documentElement.clientWidth ||
        document.body.clientWidth;

      return width < 798;
    },
    sizes(){
      let sizes = {}
      
       if(this.size === 'large') {
        sizes['width'] = '900px';
        sizes['height'] = '900px';
        sizes['fontSize'] = '18px';
       }
       else if(this.size === 'medium') {
        sizes['width'] = '700px';
        sizes['height'] = '700px';
        sizes['fontSize'] = '18px';

       }
       else if(this.size === 'small') {
        sizes['width'] = '400px';
        sizes['height'] = '400px';
        sizes['fontSize'] = '14px';

       }
       else if(this.size === 'thin') {
        sizes['width'] = '300px';
        sizes['height'] = '300px';
        sizes['fontSize'] = '12px';

       }
       else {
        sizes['width'] = '100%';
        sizes['height'] = '100%';
       }
       return sizes;
    },
    dateFormat() {
      return moment(this.date).format(this.config.formatDate);
    },
  },
  methods: {
    next() {
      let date = moment(this.date).add(1, "months");
      let month = date.format("MM");
      this.date = `${this.year}-${month}-${this.day}`;
    },
    previous() {
      let date = moment(this.date).subtract(1, "months");
      let month = date.format("MM");
      this.date = `${this.year}-${month}-${this.day}`;
    },
    current() {
      this.date = moment().valueOf();
    },
    getAllDaysOfMonth() {
      let daysOfMonth = [];
      let count = 0;

      // Calendario do mês
      let firstDay = moment(`${this.year}-${this.month}-01`);
      let weekFirstDayIndex = this.config.weekdays.indexOf(
        firstDay.format("dddd")
      );
      let lastDay = firstDay.endOf("month").format("DD");

      //Mes anterior
      let monthPrevious = moment(this.date).subtract(1, "months").format("MM");
      let firstDayPrevious = moment(`${this.year}-${monthPrevious}-01`);
      let lastDayPrevious = firstDayPrevious.endOf("month").format("DD");

      //Proximo Mes
      let firstDayNext = 1;

      let countPreviuos =
        parseInt(lastDayPrevious) - parseInt(weekFirstDayIndex);
      let countDaysPreviousShow = 0;
      for (countPreviuos; countPreviuos < lastDayPrevious; countPreviuos++) {
        countDaysPreviousShow++;
        daysOfMonth.push({
          value: countPreviuos,
          currentMonth: false,
        });
      }

      for (count; count < lastDay; count++) {
        daysOfMonth.push({
          value: count + 1,
          currentMonth: true,
        });
      }

      let dif =
        weekFirstDayIndex < 5 && count > 28
          ? 35 - (countDaysPreviousShow + count)
          : 42 - (countDaysPreviousShow + count);

      count = 1;
      for (count; count <= dif; count++) {
        daysOfMonth.push({
          value: firstDayNext,
          currentMonth: false,
        });
        firstDayNext++;
      }

      return daysOfMonth;
    },
  },
};
</script>
<style>
.sweet-header {
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
  box-sizing: border-box;
}

.sweet-date {
  font-weight: 900;
  color: rgb(55, 127, 190);
  font-size: 1.5rem;
  margin-left: 20px;
  background-color: aliceblue;
  padding: 10px;
  box-sizing: border-box;
  font-size: 18px;
}

.sweet-btn {
  padding:10px;
  margin: 0 5px;
  background-color: aliceblue;
  border: none;
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  font-weight: 900;
  color: rgb(55, 127, 190);
  transition: 0.3s;
  box-sizing: border-box;
  font-size:v-bind(sizes.fontSize)
}

.sweet-btn:hover {
  background-color: rgb(207, 221, 233);
}
.sweet-btn:active {
  box-shadow: inset 0 4px 4px rgba(0, 0, 0, 0.25);
}
.sweet-vue-calendar {
  width: v-bind(sizes.width);
  height: v-bind(sizes.height);
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}
.sweet-days-month {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: 40px repeat(6, 1fr);
  height: 100%;
  grid-gap: 5px;
  padding: 10px;
  color: rgb(55, 127, 190);
  font-weight: 700;
}
.sweet-day {
  height: 100%;
  background-color: aliceblue;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.25);
}
.sweet-day:hover {
  background-color: rgb(230, 239, 247);
}
.sweet-day-off {
  background: white;
}
.sweet-week {
  background-color: aliceblue;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.25);
  display: flex;
  justify-content: center;
  align-items: center;
}
.sweet-week:hover {
  background-color: rgb(230, 239, 247);
}
</style>
