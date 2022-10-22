<template>
  <div class="main row justify-center items-center">
    <div class="calendar row items-between">
      <div class="col-12">
        <div class="title row justify-center q-mt-md">{{ Month }}</div>
      </div>
      <div class="col-12">
        <div class="row justify-center">
          <div class="col-10">
            <div class="day-in-week">
              <div class="col-1" v-for="item in days" :key="item">
                <div class="row justify-center">
                  {{ item }}
                </div>
              </div>
            </div>
            <div class="day-in-week">
              <div
                class="q-mt-xs day-in-month col-1 row justify-center"
                v-for="day in DaysofMonthArr"
                :key="day"
                :class="day.active ? 'active' : 'pasive'"
              >
                {{ day.day }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-12">
        <div class="row justify-center">
          <div class="col-10">
            <div class="row justify-between">
              <div class="after" @click="minus()">after</div>
              <div>{{ fullYear }}</div>
              <div class="before" @click="plus()">before</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";
import { date, QInnerLoading } from "quasar";
const timeStamp = Date.now();
let Month = date.formatDate(timeStamp, "M");
let Year = date.formatDate(timeStamp, "YYYY");
let dayOfMonth = date.formatDate(timeStamp, "DD");
console.log(dayOfMonth, Month, Year);
let Monthofchenging = ref(0);
let DaysofMonthArr = ref([]);
onMounted(() => {
  daysInLastMonth();
});
let days = ["Mon", "tue", "Wed", "Thu", "Fri", "Sat", "sun"];
let getDatePrev = ref(new Date(Year, +Month));

watch(Month, () => {
  daysInLastMonth();
  console.log(1234);
});
let getDate = ref(new Date(Year, +Month - 1));

let getDateLast = ref(new Date(Year, +Month - 2));

console.log(getDatePrev.value);
let DaysOfThisMonth = ref(
  (getDatePrev.value - getDate.value) / 1000 / 3600 / 24
);

function plus() {
  +Month++;
  fullYear.value = new Date(Year, Month - 1, dayOfMonth).getFullYear();
  getDatePrev.value = new Date(Year, +Month);
  getDate.value = new Date(Year, +Month - 1);
  DaysOfThisMonth.value =
    (getDatePrev.value - getDate.value) / 1000 / 3600 / 24;
  console.log();
  getDateLast.value = new Date(Year, +Month - 2);
  daysInLastMonth();
}
function minus() {
  +Month--;
}
let daysInlast = ref((getDate.value - getDateLast.value) / 1000 / 3600 / 24);
let fullYear = ref();

console.log(DaysOfThisMonth.value);
function daysInLastMonth() {
  DaysofMonthArr.value = [];
  console.log(getDate.value.getDay());
  for (
    let i = getDate.value.getDay() == 0 ? 5 : getDate.value.getDay() - 2;
    i >= 0;
    i--
  ) {
    DaysofMonthArr.value.push({ day: daysInlast.value - i, active: false });
  }
  DaysInMonth();
  DaysInNextMonth();
}
function DaysInMonth() {
  console.log(DaysOfThisMonth.value);
  for (let i = 0; i < DaysOfThisMonth.value; i++) {
    DaysofMonthArr.value.push({ day: i + 1, active: true });
  }
}
console.log(new Date(Year, +Month + 1).getDay());
function DaysInNextMonth() {
  console.log(getDate.value.getDay());
  let count = 0;
  if (getDate.value.getDay() === 6) {
    count = 6;
  } else count = getDate.value.getDay() + 2;
  for (let i = 0; i < count; i++) {
    DaysofMonthArr.value.push({ day: i + 1, active: false });
  }
  console.log(DaysofMonthArr.value);
}
</script>
<style coped>
.main {
  height: 100vh;
  background-image: linear-gradient(
    to bottom right,
    rgba(197, 102, 73, 0.7),
    rgba(166, 168, 11, 0.1),
    rgba(27, 13, 233, 0.2),
    rgba(99, 197, 33, 0.4)
  );
}
.calendar {
  width: 60%;
  height: 70%;
  background-color: rgb(95, 165, 67);
  opacity: 0.6;
  border-top-left-radius: 10%;
  border-top-right-radius: 10%;
}
.title {
  font-size: 30px;
  color: aliceblue;
  font-weight: bold;
}
/* .day-in-month {
  margin-right: 40px;
} */
.day-in-week {
  color: #fff;
  font-size: 25px;
  display: grid;
  grid-template-columns: repeat(7, auto);
}
.pasive {
  color: #fff;
  opacity: 0.5;
}
.active {
  cursor: pointer;
  color: #fff;
}
.after {
  font-size: 45px;
  color: blue;
}
.before {
  font-size: 45px;
  color: red;
}
</style>
