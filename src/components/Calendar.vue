<template>
<div>
  <table class="table">
    <thead>
      <tr>
        <td>
          <button @click="decrease"></button>
        </td>
        <td colspan="5">{{monthes[month]}} {{year}}</td>
        <td>
          <button @click="increase"></button>
        </td>
      </tr>
      <tr>
        <td v-for="d in day" :key="d">{{d}}</td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="week in calendar()" :key="week">
        <td @click="event => showDay(event)" class="day" v-for="day in week" :key="day" :style="{'color': day.weekend, 'background-color': day.current}">{{day.index}}</td>
      </tr>
    </tbody>
  </table>
  <p v-if="checked">You choosen {{checkedDay}} {{monthes[month]}} {{year}}</p>
</div>
</template>

<script>
export default {
  name: "Calendar",
  data(){
    return{
      month: new Date().getMonth(),
      year: new Date().getFullYear(),
      dFirstMonth: '1',
      day: ["Mn","Tu","We","Th","Fr","Sa","Su"],
      monthes: ["January","February","March","April","May","June","July","August","September","October","November","December"],
      date: new Date(),
      checked: false,
      checkedDay: '',
    }
  },
  mounted(){
    /*let dlast = new Date(this.year, this.month+1, 0).getDate();*/
  },
  methods:{
    calendar: function (){
      let days = [];
      let week = 0;
      days[week] = [];
      let dlast = new Date(this.year, this.month+1,0).getDate();
      for (let i=1; i<=dlast; i++){
        if (new Date(this.year, this.month,i).getDay() != this.dFirstMonth){
          let a = {index: i};
          days[week].push(a);
          if (i == new Date().getDate() && this.year == new Date().getFullYear() && this.month == new Date().getMonth()) { a.current = '#747ae6'}
          if (new Date(this.year, this.month, i).getDay() == 6 || new Date(this.year, this.month, i).getDay() == 0) { a.weekend = '#ff0000'}
        }else {
          week++;
          days[week] = [];
          let a = {index:i};
          days[week].push(a);
        }
      }
      if (days[0].length>0){
        for (let i = days[0].length; i<7; i++){
          days[0].unshift('');
        }
      }
      /*this.dayChange;*///для смены языка
      return days;
    },
    decrease: function (){
      this.month--;
      this.checked = false;
      if (this.month<0){
        this.month = 12;
        this.month--;
        this.year--;
      }
    },
    increase: function (){
      this.month++;
      this.checked = false;
      if (this.month>11){
        this.month = -1;
        this.month++;
        this.year++;
      }
    },
    showDay(event){
      this.checkedDay = event.target.innerHTML;
      if (this.checkedDay != ''){
        this.checked = true;
      }
    }
  }
}
</script>

<style scoped lang="less">
#app {
  margin-top: 100px;
  width: 400px;
  margin-left: auto;
  margin-right: auto;
}
.table {
  border-collapse: collapse;
  float: left;
  width: 180px;
  table-layout: fixed;
}
.format-week {
  float:right;
}
.table td {
  text-align:center;
}
.table thead tr:last-child {
  background-color: #dedada;
}
.day{
  cursor: pointer;
  &:hover{
    background: aqua;
  }
  &:focus{
    background: brown;
  }
}
.selected{
  background: brown;
}
</style>