<style>
.datepicker{
  position: absolute;
  top: 100%;
  width: 315px;
  z-index: 5;
  background-color: #fff;
  box-shadow:0 14px 45px rgba(0,0,0,0.25), 0 10px 18px rgba(0,0,0,0.22);
}

.datepicker_header{
  background-color: #0097a7;
  color: #FFF;
  padding: 20px;
  height: 100px;
}

.datepicker_year{
  opacity: 0.7;
  margin-bottom: 10px;
  line-height: 16px;
}

.datepicker_date{
  font-size: 32px;
  line-height: 32px;
}
.datepicker_week{
  font-size: 12px;
  line-height: 12px;
  color: rgba(0,0,0,0.8);
  padding: 0 14px;
}

.datepicker_weekday{
  float: left;
  width: 41px;
  text-align: center;
}

.datepicker_day{
  width: 41px;
  height: 41px;
  float: left;
}
</style>

<template>
  <div class="datepicker">
    <div class="datepicker_header">
      <div class="datepicker_year">
        {{year}}
      </div>
      <div class="datepicker_date">
        {{date_formatted}}
      </div>
    </div>
    <div class="datepicker_week">
      <div v-for="day in days" track-by="$index" class="datepicker_weekday">
        {{day}}
      </div>
    </div>
    <div class="datepicker_days">
      <div class="datepicker_day" style=":width:(month.getWeekStart() * 41) + 'px'">
      </div>
      <div class="datepicker_day">
        1
      </div>
    </div>
  </div>
</template>
<script>
/* eslint-disable */
import Month from '../modules/month'
export default {
  props: ['date'],
  data(){
    return{
      days: ['L','M','M','J','V','S','D'],
      month: new Month(this.date.month(),this.date.year())
    }
  },
  computed: {
    year(){
      return this.date.format('YYYY')
    },
    date_formatted(){
      return this.date.format('dddd DD MMM')
    }
  }
}
</script>
