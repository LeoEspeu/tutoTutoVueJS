<style>
.datepicker_container{
  position: relative;
}
</style>
<template>
  <div class="datepicker_container">
    <input type="text" :value="date_formated">
    <input type="text" :name="name" :value="date_raw">
    <datepicker-agenda v-bind:date="date"></datepicker-agenda>
  </div>
</template>
<script>
/* eslint-disable */
import moment from 'moment'
import DatepickerAgendaComponent from './datepickerAgenda'

moment.locale('fr')

export default{
  components:{
    'datepicker-agenda': DatepickerAgendaComponent
  },
  props: {
    value: {type: String, required: true},
    format: {type: String, default: 'YYYY-MM-DD'},
    name: {type: String}
  },
  data: function () {
    return {
      date: moment(this.value, 'YYYY-MM-DD')
    }
  },
  computed: {
    date_formated: function () {
      return this.date.format(this.format)
    },
    date_raw: function () {
      return this.date.format('YYYY-MM-DD')
    }
  }
}
</script>
