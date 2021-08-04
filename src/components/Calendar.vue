<template>
<div class="content">
  <table class="table">
    <thead>
      <tr>
        <td>
          <input class="button" type="button" value="<" @click="decrease">
        </td>
        <td colspan="5">{{monthes[month]}} {{year}}</td>
        <td>
          <input class="button" type="button" value=">" @click="increase">
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
  <div class="settingForm">
    <form class="defaultDate" @submit.prevent="">
      <label v-if="english" for="defaultDate">Enter date: </label>
      <label v-if="!english" for="defaultDate">Введите дату:</label>
      <input id="defaultDate" type="date" required pattern="[0-9]{4}-[0-9]{2}-[0-9]{2}" v-model="startDate">
      <input class="settingButton" v-if="english" type="submit" @click="getStartDate" value="Set date">
      <input class="settingButton" v-if="english" type="submit" @click="getRealDate" value="Synchronize date online">
      <input class="settingButton" v-if="!english" type="submit" @click="getStartDate" value="Установить дату">
      <input class="settingButton" v-if="!english" type="submit" @click="getRealDate" value="Синхронизировать дату">
    </form>
    <form class="defaultLang" @submit.prevent="">
      <input class="settingButton" v-if="english" type="button" value="Change language" @click="changeLang">
      <input class="settingButton" v-if="!english" type="button" value="Сменить язык" @click="changeLang">
    </form>
    <p class="choosenDate" v-if="checked&&english">You choosen {{checkedDay}}.{{month+1}}.{{year}}</p>
    <p class="choosenDate" v-if="checked&&!english">Вы выбрали {{checkedDay}}.{{month+1}}.{{year}}</p>
  </div>
</div>
</template>

<script>
export default {
  name: "Calendar",
  data(){
    return{
      month: new Date().getMonth(),
      year: new Date().getFullYear(),
      currentMonth:'',
      currentYear:'',
      dFirstMonth: '1',
      defaultDay:["Mn","Tu","We","Th","Fr","Sa","Su"],
      defaultMonthes:["January","February","March","April","May","June","July","August","September","October","November","December"],
      day: ["Mn","Tu","We","Th","Fr","Sa","Su"],
      monthes: ["January","February","March","April","May","June","July","August","September","October","November","December"],
      dayRus: ["Пн","Вт","Ср","Чт","Пт","Сб","Вс"],
      monthesRus: ["Январь","Февраль","Март","Апрель","Май","Июнь","Июль","Август","Сентябрь","Октябрь","Ноябрь","Декабрь"],
      date: '',
      checked: false,
      changed: false,
      checkedDay: '',
      startDate: '',
      current:'',
      english: true
    }
  },
  mounted(){
    this.date = new Date();
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
            if (i == new Date().getDate() && this.year == new Date().getFullYear() && this.month == new Date().getMonth() && this.changed == false) { a.current = '#747ae6'}
            if (this.changed && i==this.current && this.month==this.currentMonth && this.year==this.currentYear) { a.current = '#747ae6'}
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
    },
    getStartDate(){
        let value = this.startDate;
        this.checked = false;
        let year = parseInt(value.slice(0,4));
        let month = parseInt(value.slice(5,7));
        let day = parseInt(value.slice(8,10));
        let date = new Date(`${year}/${month}/${day}`);
        this.date = date;
        this.year = this.currentYear = year;
        this.month = this.currentMonth = month-1;
        this.current = day;
        this.changed = true;
        this.calendar();
    },
    changeLang(){
      this.english = !this.english;
      if(this.english){
        this.day = this.defaultDay;
        this.monthes = this.defaultMonthes;
      }else{
        this.day = this.dayRus;
        this.monthes = this.monthesRus;
      }
    },
    getRealDate(){
      this.date = new Date();
      this.changed = false;
      this.month = new Date().getMonth();
      this.year = new Date().getFullYear();
      this.calendar();
    }
  },
}
</script>

<style scoped lang="less">
#app {
  margin-top: 100px;
  width: 400px;
  margin-left: auto;
  margin-right: auto;
}
.button{
  background: transparent;
  border: none;
  cursor: pointer;
  &:hover{
    transform: scale(1.5);
  }
}
.settingButton{
  width: 160px;
}
.content{
  margin: 0 auto;
  text-align: center;
  display: flex;
}
.settingForm{
  display: flex;
  flex-direction: column;
}
.table {
  border-collapse: collapse;
  float: left;
  width: 180px;
  height: 180px;
  table-layout: fixed;
  margin-right: 20px;
}
.defaultDate{
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
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
    outline: 1px solid black;
  }
}
.choosenDate{
  font-size: 14px;
}
</style>