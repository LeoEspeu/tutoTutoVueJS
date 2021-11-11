<style>
.datepicker_container{
  position: relative;
}
</style>
<template>
  <div class="datepicker_container">
    <input type="text" :value="date_formated" @click="showDatepicker()">
    <input type="hidden" :name="name" :value="date_raw">
    <datepicker-agenda :date="date" :visible="isVisible" @update-date="update" @cancel="hideDatepicker()"></datepicker-agenda>
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
      isVisible: false,
      date: moment(this.value, 'YYYY-MM-DD')
    }
  },
  methods: {
    update(date) {
      this.date = date;
      this.hideDatepicker()
    },
    showDatepicker: function (){
      this.isVisible = true;
      setTimeout(() => document.addEventListener('click',this.hideDatepicker),0)
    },
    hideDatepicker: function (){
      this.isVisible = false;
      document.removeEventListener('click',this.hideDatepicker)
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
