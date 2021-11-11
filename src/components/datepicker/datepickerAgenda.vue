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
  position: relative;
  opacity: 0.7;
  height: 16px;
  margin-bottom: 10px;
  line-height: 16px;
}

.datepicker_date{
  position: relative;
  overflow: hidden;
  height: 32px;
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

.datepicker_days{
  position: relative;
  overflow: hidden;
  margin: 14px 14px 0;
  height: 205px;
  transition: height 0.3s;
}

.datepicker_days.has-6-weeks{
  height: 246px;
}

.datepicker_day{
  position: relative;
  width: 41px;
  height: 41px;
  float: left;
  text-align: center;
  line-height: 41px;
  cursor: pointer;
  transition: color 450ms cubic-bezier(0.23,1,0.32,1);
}

.datepicker_day_effect{
  position: absolute;
  top: 2px;
  left: 2px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: rgb(0,151,167);
  transition: all 450ms cubic-bezier(0.23,1,0.32,1);
  transform: scale(0);
  opacity: 0;
}

.datepicker_day_text{
  position: relative;
}

.datepicker_day:hover{
  color: #FFF;
}

.datepicker_day:hover .datepicker_day_effect{
  transform: scale(1);
  opacity: 0.6;
}

.datepicker_day.selected{
  color: #FFF;
}

.datepicker_day.selected .datepicker_day_effect{
  transform: scale(1);
  opacity: 1;
}

.datepicker_controls{
  position: relative;
  height: 56px;
  line-height: 56px;
  text-align: center;
}

.datepicker_controls button{
  position: relative;
  border: none;
  background-color: transparent;
  user-select: none;
  outline: none;
  cursor: pointer;
  width: 56px;
  height: 56px;
}

.datepicker_next{
  float: right;
}

.datepicker_prev{
  float: left;
}

.datepicker_actions{
  text-align: right;
  padding: 8px;
}

.datepicker_actions button{
  border: none;
  background-color: transparent;
  display: inline-block;
  cursor: pointer;
  outline: none;
  color: #00bcd4;
  font-size: 14px;
  text-transform: uppercase;
  font-weight: 500;
  min-width: 88px;
  line-height: 36px;
  text-align: center;
  -webkit-appearance: none;
  transition: all 0.3s;
}

.datepicker_actions button:hover{
  background-color: rgba(153,153,153,0.2);
}

.datepicker_month{
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.datepicker-slide-transition{
  opacity: 1;
  transition: all 0.3s;
  transform: translateY(0);
}

.datepicker-slide-leave, .datepicker-slide-enter{
  opacity: 0;
  transform: translateY(-15px);
}

.slidev-transition{
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  transition: all 0.3s;
  transform: translateX(0);
  opacity: 1;
}

.slide-leave{
  transform: translateX(-100%);
  opacity: 1;
}

.slide-enter{
  transform: translateX(100%);
  opacity: 0;
}

</style>

<template>
  <div class="datepicker" v-if="visible" transition="datepicker-slide" @click.stop>
    <div class="datepicker_header">
      <div class="datepicker_year">
        {{year}}
      </div>
      <div class="datepicker_date">
        {{date_formatted}}
      </div>
    </div>
    <div class="datepicker_controls">
      <div class="datepicker_month">{{ month.getFormatted() }}</div>
      <button class="datepicker_next" @click="nextMonth()">></button>
      <button class="datepicker_prev" @click="prevMonth()"><</button>
    </div>
    <div class="datepicker_week">
      <div v-for="day in days" track-by="$index" class="datepicker_weekday">
        {{day}}
      </div>
    </div>
    <div class="datepicker_days" :class="classWeeks">
      <div v-for="month in [month]" transition="slidev">
        <div class="datepicker_day" :class="{'active':'isActive'}" :style="'width:' + (month.getWeekStart() * 41) + 'px'">
        </div>
        <div class="datepicker_day" @click="selectDate(day)" v-for="day in month.getDays()" :class="isSelected(day) ? 'selected' : ''">
          <span class="datepicker_day_effect"></span>
          <span class="datepicker_day_text">{{ day.format('D') }}</span>
        </div>
      </div>
    </div>
    <div class="datepicker_actions">
      <button @click="cancel()">Annuler</button>
      <button @click="submit()">Ok</button>
    </div>
  </div>
</template>
<script>
/* eslint-disable */
import Month from '../../modules/month'
export default {
  props: {
    date: {},
    visible: {type: Boolean, default: true}
  },
  data(){
    return{
      days: ['L','M','M','J','V','S','D'],
      month: new Month(this.date.month(),this.date.year())
    }
  },
  methods: {
    isSelected: function (day){
      return this.date.unix() === day.unix()
    },
    selectDate: function (day){
      this.date = day.clone()
    },
    nextMonth: function (){
      let month = this.month.month + 1
      let year = this.month.year
      if(month > 11){
        year += 1
        month = 0
      }
      this.month = new Month(month,year)
    },
    prevMonth: function (){
      let month = this.month.month - 1
      let year = this.month.year
      if(month < 0){
        year -= 1
        month = 11
      }
      this.month = new Month(month,year)
    },
    submit: function (){
      this.$emit("update-date", this.date)
    },
    cancel: function (){
      this.$emit("cancel")
    }
  },
  computed: {
    year(){
      return this.date.format('YYYY')
    },
    classWeeks(){
      return 'has-' + this.month.getWeeks() + '-weeks'
    },
    date_formatted(){
      return this.date.format('dddd DD MMM')
    }
  }
}
</script>
