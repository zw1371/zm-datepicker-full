<template>
<div  @mouseenter="changeIconClose()" @mouseleave="changeIconDate()" @click.stop="panelClick" class="vue-datepicker">
  <i v-show="iconState" @click="clearData()" class="zm_data_ic" :class="iconClass"></i>
  <span v-show="this.type == 'week'"  @click="startChoice" class="week-input">{{weekShortText}}</span>
  <span v-show="this.type == 'month'"  @click="startChoice" class="month-input">{{monthShortText}}</span>
  <span v-show="this.type == 'day'"  @click="startChoice" class="day-input">{{dayShortText}}</span>
  <span v-show="this.type == 'hour'"  @click="startChoice" class="hour-input">{{hourShortText}}</span>
  <span v-show="this.type == 'minute'"  @click="startChoice" class="minute-input">{{minuteShortText}}</span>
  <input  readonly="readonly" type="text" class="form-control  backWhite" autocomplete="off" disableautocomplete :name="field" :id="field" :value="value" :placeholder="placeholder" @focus="startChoice" @click="startChoice"
      ref="input">
  <div class="vue-datepicker-panels" v-show="dayPanelIsShow || monthPanelIsShow || weekPanelIsShow || hourPanelIsShow || minutePanelIsShow">
    <!--年月选择-->
    <div class="vue-datepicker-panel" v-show="monthPanelIsShow">
      <div class="vue-datepicker-month">
        <a class="vue-datepicker-prev" @click="prevYear"> &lt; </a>
        <span>{{ year }}年</span>
        <a class="vue-datepicker-next" @click="nextYear"> &gt; </a>
      </div>
      <table class="vue-datepicker-tb2">
        <col width="33%" />
        <col width="33%" />
        <col width="33%" />
        <tbody>
          <tr v-for="season in monthArr" track-by="$index">
            <td v-for="m in season" track-by="$index" @click="choiceMonth(m.id, $event)" :class="classMonth(m.id)">
              <span>{{ m.name }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--周选择-->
    <div class="vue-datepicker-panel" v-show="weekPanelIsShow">
      <div class="vue-datepicker-month">
        <a class="vue-datepicker-prev" @click="prevYear"> &lt; </a>
        <span>{{ year }}年</span>
        <a class="vue-datepicker-next" @click="nextYear"> &gt; </a>
      </div>
      <div class="week-content">
        <div v-for="(item,index) in weekData" :key="index" :class="classWeek(item.weekIndex)" @click="choiceWeek(item.weekIndex,$event,item.weekStartDate,item.weekEndDate,item.weekShortText)" :id="'week'+item.weekIndex" class="week-item">
          {{item.weekText}}
        </div>
      </div>
    </div>
    <!--日期选择-->
    <div class="vue-datepicker-panel" v-show="dayPanelIsShow">
      <div class="vue-datepicker-month">
        <a class="vue-datepicker-prev" @click="prevMonth"> &lt; </a>
        <span class="vue-datepicker-btn" @click="startChoiceMonth">{{ year }}年 {{ month }}月</span>
        <a class="vue-datepicker-next" @click="nextMonth"> &gt; </a>
      </div>
      <table class="vue-datepicker-tb">
        <thead>
          <tr>
            <th v-for="d in langConf.week">{{ d }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(m, mIndex) in monthDays" track-by="$index">
            <td v-for="(d, dIndex) in m" track-by="$index" @click="choiceDay(d, $event)" :class="classDay(d)">
              <span>{{ d }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--小时选择-->
    <div class="vue-datepicker-panel" v-show="hourPanelIsShow">
      <div class="vue-datepicker-month">
        <a class="vue-datepicker-prev" @click="prevDay"> &lt; </a>
        <span class="vue-datepicker-btn" @click="startChoiceDate">{{ year }}年 {{ month }}月 {{day}}日</span>
        <a class="vue-datepicker-next" @click="nextDay"> &gt; </a>
      </div>
      <table class="vue-datepicker-tb2">
        <tbody>
            <thead></thead>
          <tr>
             <td :class="classHour(0)"><span @click="choiceHour(0, $event)"> 0:00 </span></td>
             <td :class="classHour(1)"><span @click="choiceHour(1, $event)"> 1:00 </span></td>
             <td :class="classHour(2)"><span @click="choiceHour(2, $event)"> 2:00 </span></td>
             <td :class="classHour(3)"><span @click="choiceHour(3, $event)"> 3:00 </span></td>
          </tr>
          <tr>
              <td :class="classHour(4)"><span @click="choiceHour(4, $event)"> 4:00 </span></td>
              <td :class="classHour(5)"><span @click="choiceHour(5, $event)"> 5:00 </span></td>
              <td :class="classHour(6)"><span @click="choiceHour(6, $event)"> 6:00 </span></td>
              <td :class="classHour(7)"><span @click="choiceHour(7, $event)"> 7:00 </span></td>
          </tr>
          <tr>
              <td :class="classHour(8)"><span @click="choiceHour(8, $event)"> 8:00 </span></td>
              <td :class="classHour(9)"><span @click="choiceHour(9, $event)"> 9:00 </span></td>
              <td :class="classHour(10)"><span @click="choiceHour(10, $event)"> 10:00 </span></td>
              <td :class="classHour(11)"><span @click="choiceHour(11, $event)"> 11:00 </span></td>
          </tr>
          <tr>
              <td :class="classHour(12)"><span @click="choiceHour(12, $event)"> 12:00 </span></td>
              <td :class="classHour(13)"><span @click="choiceHour(13, $event)"> 13:00 </span></td>
              <td :class="classHour(14)"><span @click="choiceHour(14, $event)"> 14:00 </span></td>
              <td :class="classHour(15)"><span @click="choiceHour(15, $event)"> 15:00 </span></td>
          </tr>
          <tr>
              <td :class="classHour(16)"><span @click="choiceHour(16, $event)"> 16:00 </span></td>
              <td :class="classHour(17)"><span @click="choiceHour(17, $event)"> 17:00 </span></td>
              <td :class="classHour(18)"><span @click="choiceHour(18, $event)"> 18:00 </span></td>
              <td :class="classHour(19)"><span @click="choiceHour(19, $event)"> 19:00 </span></td>
          </tr>
          <tr>
              <td :class="classHour(20)"><span @click="choiceHour(20, $event)"> 20:00 </span></td>
              <td :class="classHour(21)"><span @click="choiceHour(21, $event)"> 21:00 </span></td>
              <td :class="classHour(22)"><span @click="choiceHour(22, $event)"> 22:00 </span></td>
              <td :class="classHour(23)"><span @click="choiceHour(23, $event)"> 23:00 </span></td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--分钟选择-->
    <div class="vue-datepicker-panel" v-show="minutePanelIsShow">
      <div class="vue-datepicker-month">
        <a class="vue-datepicker-prev" @click="prevHour"> &lt; </a>
        <span class="vue-datepicker-btn" @click="startChoiceHour">{{ year }}年 {{ month }}月 {{day}}日 {{hour}}时</span>
        <a class="vue-datepicker-next" @click="nextHour"> &gt; </a>
      </div>
      <table class="vue-datepicker-tb2">
        <tbody>
            <thead></thead>
          <tr>
             <td :class="classMinute(0)"><span @click="choiceMinute(0, $event)"> 00 </span></td>
             <td :class="classMinute(5)"><span @click="choiceMinute(5, $event)"> 05 </span></td>
             <td :class="classMinute(10)"><span @click="choiceMinute(10, $event)"> 10 </span></td>
             <td :class="classMinute(15)"><span @click="choiceMinute(15, $event)"> 15 </span></td>
          </tr>
          <tr>
              <td :class="classMinute(20)"><span @click="choiceMinute(20, $event)"> 20 </span></td>
              <td :class="classMinute(25)"><span @click="choiceMinute(25, $event)"> 25 </span></td>
              <td :class="classMinute(30)"><span @click="choiceMinute(30, $event)"> 30 </span></td>
              <td :class="classMinute(35)"><span @click="choiceMinute(35, $event)"> 35 </span></td>
          </tr>
          <tr>
              <td :class="classMinute(40)"><span @click="choiceMinute(40, $event)"> 40 </span></td>
              <td :class="classMinute(45)"><span @click="choiceMinute(45, $event)"> 45 </span></td>
              <td :class="classMinute(50)"><span @click="choiceMinute(50, $event)"> 50 </span></td>
              <td :class="classMinute(55)"><span @click="choiceMinute(55, $event)"> 55 </span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
</template>

<script>
export default {
  props: {
    field: {
      type: String,
      default: ''
    },
    value: {
      type: String,
      default: '',
      twoWay: true
    },
    maxdate:{
      type: String,
      default: ''
    },
    mindate:{
      type: String,
      default: ''
    },
    format: {
      type: String,
      default: 'yyyy-mm-dd'
    },
    placeholder: {
      type: String,
      default: ''
    },
    weektype:{
      type: String,
      default: 'start'
    },
    type:{
      type: String,
      default: 'minute' //minute表示选择到分钟,hour表示选择到小时,day表示选择到天,month表示选择到月
    },
    conf: {
      type: Object,
      default: function() {
        return {};
      }
    }
  },
  data() {
    let {
      value,
      conf
    } = this,
    dateObj, year, month, day, hour, minute, second,
    langConf = {
      week: ['日', '一', '二', '三', '四', '五', '六'],
      month: ['一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一月', '十二月']
    };
    if (value) {
      dateObj = new Date(value.replace(/-/g, "/"));
    } else {
      dateObj = new Date();
    }
    if (conf) {
      langConf = Object.assign(langConf, conf);
    }
    year = dateObj.getFullYear();
    month = dateObj.getMonth()+1;
    day = dateObj.getDate();
    hour = dateObj.getHours();
    minute = dateObj.getMinutes();
    second = dateObj.getSeconds();

    return {
      langConf,
      dayPanelIsShow: false,
      weekPanelIsShow:false,
      monthPanelIsShow: false,
      hourPanelIsShow:false,
      minutePanelIsShow:false,
      isMouseOver: false,
      iconClass:'el-icon-date',
      iconState:false,
      week:null,
      weekShortText:"",
      monthShortText:"",
      dayShortText:"",
      hourShortText:"",
      minuteShortText:"",
      weekData:[],
      year,
      month,
      day,
      hour,
      minute,
      second
    };
  },
  watch:{
    type(val){
      if(val == "week"){
        this.weekData = getWeekData(this.year);
        let time = new Date(this.value);
        this.weekShortText = this.getWeekShortText(time.getTime());
      }
    },
    value(val){
      if(!!val){
        let time = new Date(val);
        let timeDate = this.dateTimeFormat(time.getFullYear(),time.getMonth()+1,time.getDate(),time.getHours(),time.getMinutes(),time.getSeconds(),'yyyy-MM-dd hh:mm:ss');
        this.monthShortText = this.dateTimeFormat(time.getFullYear(),time.getMonth()+1,time.getDate(),time.getHours(),time.getMinutes(),time.getSeconds(),'yyyy-MM');
        this.dayShortText = this.dateTimeFormat(time.getFullYear(),time.getMonth()+1,time.getDate(),time.getHours(),time.getMinutes(),time.getSeconds(),'yyyy-MM-dd');
        this.hourShortText = this.dateTimeFormat(time.getFullYear(),time.getMonth()+1,time.getDate(),time.getHours(),time.getMinutes(),time.getSeconds(),'yyyy-MM-dd hh');
        this.minuteShortText = this.dateTimeFormat(time.getFullYear(),time.getMonth()+1,time.getDate(),time.getHours(),time.getMinutes(),time.getSeconds(),'yyyy-MM-dd hh:mm');
        this.weekShortText = this.getWeekShortText(time.getTime());
        this.year = time.getFullYear();
        this.month = time.getMonth()+1;
        this.day = time.getDate();
        this.hour = time.getHours();
        this.minute = time.getMinutes();
        this.second = time.getSeconds();
      }else{
        this.monthShortText = this.placeholder;
        this.dayShortText = this.placeholder;
        this.hourShortText = this.placeholder;
        this.minuteShortText = this.placeholder;
        this.weekShortText = this.placeholder;
        this.year = -1;
        this.month = -1;
        this.day = -1;
        this.hour = -1;
        this.minute = -1;
        this.second = -1;
      }
    }
  },
  mounted: function() {
      if(this.type == "week"){
        this.weekData = getWeekData(this.year);
      }
      let that = this;
      document.body.addEventListener("click",function(){
          that.immEndChoice();
      });
      this.weekShortText = this.placeholder;
      this.monthShortText = this.placeholder;
      this.dayShortText = this.placeholder;
      this.hourShortText = this.placeholder;
      this.minuteShortText = this.placeholder;

  },
  computed: {
    //	日期二维数组（3*4），渲染用
    monthArr() {
      let {
        month
      } = this.langConf,
        res = [];
      for (let i = 0; i <= 3; i++) {
        let temp = [];
        for (let j = 1; j <= 3; j++) {
          let id = i * 3 + j - 1;
          temp.push({
            id,
            name: month[id]
          });
        }
        res.push(temp);
      }
      return res;
    },
    // 当前日期
    curr() {
      let {
        value
      } = this,
      dateObj,
      year = "",
      month = "",
      day = "",
      hour = "",
      minute = "",
      second = "";
      if (value) {
        dateObj = new Date(value.replace(/-/g, "/"));
      } else {
        dateObj = new Date();
      }
      year = dateObj.getFullYear();
      month = dateObj.getMonth()+1;
      day = dateObj.getDate();
      hour = dateObj.getHours();
      minute = dateObj.getMinutes();
      second = dateObj.getSeconds();
      return {
        year,
        month,
        day,
        hour,
        minute,
        second
      };
    },
    // 返回当前月的天数数组
    monthDays() {
      let {
        year,
        month
      } = this,
      dayNum;
      //month++;
      // 判断一个月有几天
      if (month == 2) {
        if (year % 4 == 0 && !(year % 100 == 0 && year % 400 != 0)) {
          dayNum = 29;
        } else {
          dayNum = 28;
        }
      } else {
        if ([1, 3, 5, 7, 8, 10, 12].includes(month)) {
          dayNum = 31;
        } else {
          dayNum = 30;
        }
      }
      // 生成对应星期的日期数组
      let firstDay = new Date(year, month - 1, 1).getDay(),
        dayArr = getDayArr(6, 7),
        row = 0,
        col = firstDay;
      for (let d = 1; d <= dayNum; d++) {
        dayArr[row][col] = d;
        if (col < 6) {
          col++;
        } else {
          col = 0;
          row++;
        }
      }

      return dayArr;
    },
    now() {
      let dateObj = new Date()
      let year = dateObj.getFullYear()
      let month = dateObj.getMonth()+1
      let day = dateObj.getDate()
      return {
        year,
        month,
        day
      };
    }
  },
  methods: {
      //点击文本框弹出日期选择
      startChoice(e) {
        if (e && e.type == 'keypress') {
          e.returnValue = false;
        }
        if (!this.dayPanelIsShow && !this.monthPanelIsShow && !this.weekPanelIsShow && !this.hourPanelIsShow && !this.minutePanelIsShow) {
          switch (this.type) {
            case "month":
              this.startChoiceMonth();
              break;
            case "week":
              this.startChoiceWeek();
              break;
            case "date":
            case "hour":
            case "minute":
              this.startChoiceDate();
              break;
            default:
              this.startChoiceDate();
          }
        }
      },
      illegal (event) {
        let target = event.target.parentNode;
        let className = target.getAttribute('class');
        if(!!className){
          return className.toString().indexOf('z-invalid') != -1;
        }
        return false;
      },
      choiceMonth(m,event) {
        if(this.illegal(event)){
          return;
        }
        this.month = m+1;
        if(this.day != 1){
          //说明是最后用户希望取办呢与最后一天
          let myDate = new Date(this.year, this.month, 0);
          this.day = myDate.getDate();
        }
        console.log("FGHDD:",this.day)

        if(this.type == "month"){
          this.monthShortText = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM');
          let day = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd');
          this.loadValue();
          this.chooseMonth(this.monthShortText,day);
          return;
        }
        this.startChoiceDate();
      },
      choiceWeek(w,event,sDate,eDate,shortText) {
        if(this.illegal(event)){
          return;
        }
        this.week = w;
        this.weekShortText = shortText;
        let startDate = {},
            startDatetime = new Date(sDate);
        startDate.year = startDatetime.getFullYear();
        startDate.month = startDatetime.getMonth()+1;
        startDate.day = startDatetime.getDate();
        startDate.hour = startDatetime.getHours();
        startDate.minute = startDatetime.getMinutes();
        startDate.second = startDatetime.getSeconds();

        let endDate = {},
            endDatetime = new Date(eDate);
        endDate.year = endDatetime.getFullYear();
        endDate.month = endDatetime.getMonth()+1;
        endDate.day = endDatetime.getDate();
        endDate.hour = endDatetime.getHours();
        endDate.minute = endDatetime.getMinutes();
        endDate.second = endDatetime.getSeconds();
        if(this.weektype == "start"){
          this.year = startDate.year;
          this.month = startDate.month;
          this.day = startDate.day;
          this.hour = 0;
          this.minute = 0;
          this.second = 0;
          console.log(this.year, this.month, this.day, this.hour, this.minute, this.second)
        }
        if(this.weektype == "end"){
          this.year = endDate.year;
          this.month = endDate.month;
          this.day = endDate.day;
          this.hour = 23;
          this.minute = 59;
          this.second = 59;
        }

        let sday = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd');
        let eday = this.dateTimeFormat(endDate.year, endDate.month, endDate.day, endDate.hour, endDate.minute, endDate.second, 'yyyy-MM-dd');


        this.loadValue();
        this.chooseWeek(sday,eday,w);
      },
      clearHolder(){
        this.weekShortText = this.placeholder;
        this.monthShortText = this.placeholder;
        this.dayShortText = this.placeholder;
        this.hourShortText = this.placeholder;
        this.minuteShortText = this.placeholder;
      },
      choiceDay(d,event) {
        if(this.illegal(event)){
          return;
        }
        this.day = d;
        if(this.type == "day"){
          this.dayShortText = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd');
          let day = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd');

          this.loadValue();
          this.chooseDay(this.dayShortText,day);
          return;
        }
        this.startChoiceHour();
      },
      choiceHour(h,event){
          if(this.illegal(event)){
            return;
          }
          this.hour = h;
          if(this.type == "hour"){
            this.hourShortText = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh');
            let day = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd');
            this.loadValue();
            this.chooseHour(this.hourShortText,day);
            return;
          }
          this.startChoiceMinute();
      },
      choiceMinute(M,event){
          if(this.illegal(event)){
            return;
          }
          this.minute = M;
          if(this.type == "minute"){
            this.minuteShortText = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh:mm');
            let day = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd');

            this.loadValue();
            this.chooseMinute(this.minuteShortText,day);
            return;
          }
      },
      chooseMonth(date,day){
        let time = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh:mm:ss');
        let data = {
          date,
          day,
          time
        };
        this.$emit('select-time','month', data);
      },
      chooseWeek(s,e,w){
        let time = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh:mm:ss');
        let data = {
          weekIndex:w,
          monday:s,
          sunday:e,
          time
        };
        this.$emit('select-time','week', data);
      },
      chooseDay(date,day){
        let time = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh:mm:ss');
        let data = {
          date,
          day,
          time
        };
        this.$emit('select-time','day', data);
      },
      chooseHour(date,day){
        let time = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh:mm:ss');
        let data = {
          date,
          day,
          time
        };
        this.$emit('select-time','hour', data);
      },
      chooseMinute(date,day){
        let time = this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second, 'yyyy-MM-dd hh:mm:ss');
        let data = {
          date,
          day,
          time
        };
        this.$emit('select-time','minute', data);
      },
      loadValue(){
        this.$emit('input', this.dateTimeFormat(this.year, this.month, this.day, this.hour, this.minute, this.second));
        this.immEndChoice();
      },
    // 年份+月份选择
    startChoiceMonth() {
      this.monthPanelIsShow = true;
      this.dayPanelIsShow = false;
      this.hourPanelIsShow = false;
      this.minutePanelIsShow = false;
      this.weekPanelIsShow = false;
    },
    startChoiceWeek() {
      this.monthPanelIsShow = false;
      this.weekPanelIsShow = true;
      this.dayPanelIsShow = false;
      this.hourPanelIsShow = false;
      this.minutePanelIsShow = false;
    },
    startChoiceDate() {
      this.monthPanelIsShow = false;
      this.dayPanelIsShow = true;
      this.hourPanelIsShow = false;
      this.minutePanelIsShow = false;
      this.weekPanelIsShow = false;
    },
    startChoiceHour() {
      this.monthPanelIsShow = false;
      this.dayPanelIsShow = false;
      this.hourPanelIsShow = true;
      this.minutePanelIsShow = false;
      this.weekPanelIsShow = false;
    },
    startChoiceMinute() {
      this.monthPanelIsShow = false;
      this.dayPanelIsShow = false;
      this.hourPanelIsShow = false;
      this.minutePanelIsShow = true;
      this.weekPanelIsShow = false;
    },
    // 样式
    classMonth(m) {
      let that = this,
        {
          month,
          year,
          curr
        } = that;
      return {
        'z-on': m == (month-1) && curr.year == year,
        'z-existed': true,
        'z-invalid':this.getZinvalid({m:m})
      };
    },
    classDay(d) {
      let that = this,
        {
          month,
          year,
          curr
        } = that;
      return {
        'z-on': curr.day == d && curr.month == month && curr.year == year,
        'z-existed': d != '',
        'z-invalid':this.getZinvalid({d:d})
      };
    },
    classHour(h){
        let that = this,
          {
            month,
            year,
            day,
            curr
          } = that;
        return {
          'z-on': curr.hour == h && curr.day == day && curr.month == month && curr.year == year,
          'z-existed': true,
          'z-invalid':this.getZinvalid({h:h})
        };
    },
    classWeek(W){
      if(this.week == W){
        return "select";
      }
      return "";
    },
    getWeekShortText(timestramp){
      let len = this.weekData.length,
          item = null,
          prevItem = null,
          shortText = "";
      for(let i = 0; i < len; i++){
        item = this.weekData[i];
        if(i > 0){
          prevItem = this.weekData[i - 1];
        }
        if(timestramp < item.weekStartDate && prevItem != null){
          this.week = prevItem.weekIndex;
          shortText = prevItem.weekShortText;
          break;
        }
      }
      return shortText;
    },
    classMinute(M){
        let that = this,
          {
            month,
            year,
            hour,
            day,
            curr
          } = that;
        return {
          'z-on': curr.minute == M && curr.hour == hour && curr.day == day && curr.month == month && curr.year == year,
          'z-existed': true,
          'z-invalid':this.getZinvalid({M:M})
        };
    },
    getZinvalid (options) {
      if(!this.maxdate && !this.mindate){
        return false;
      }
      let that = this,
        {
          year,
          month,
          day,
          hour,
          minute
        } = that;
      let regularMaxDate = "",
          regularMinDate = "",
          maxDateTime = null,
          minDateTime = null;
      if(this.maxdate  != null){
        regularMaxDate = this.maxdate.replace(/-/g, "/");
        maxDateTime = new Date(regularMaxDate).getTime();
      }
      if(this.mindate  != null){
        regularMinDate = this.mindate.replace(/-/g, "/");
        minDateTime = new Date(regularMinDate).getTime();
      }
      if(options.m != null){
        let currDate =  new Date();
        currDate.setFullYear(year);
        currDate.setMonth(options.m);
        if(!!this.maxdate){
          currDate.setDate(1);
          currDate.setHours(0);
          currDate.setMinutes(0);
          currDate.setSeconds(0);
          let currDateTime = new Date(currDate).getTime();
          if(currDateTime > maxDateTime){
            return true;
          }
        }
        if(!!this.mindate){
          let lastDate = new Date(year,options.m,0);
          currDate.setDate(lastDate.getDate());
          currDate.setHours(23);
          currDate.setMinutes(59);
          currDate.setSeconds(59);
          let currDateTime = new Date(currDate).getTime();
          if(currDateTime < minDateTime){
            return true;
          }
        }
      }
      if(options.d  != null){
        let currDate =  new Date();
        currDate.setFullYear(year);
        currDate.setMonth(month-1);
        currDate.setDate(options.d);
        if(!!this.maxdate){
          currDate.setHours(0);
          currDate.setMinutes(0);
          currDate.setSeconds(0);
          let currDateTime = new Date(currDate).getTime();
          if(currDateTime > maxDateTime){
            return true;
          }
        }
        if(!!this.mindate){
          currDate.setHours(23);
          currDate.setMinutes(59);
          currDate.setSeconds(59);
          let currDateTime = new Date(currDate).getTime();
          if(currDateTime < minDateTime){
            return true;
          }
        }
      }
      if(options.h != null){
        let currDate =  new Date();
        currDate.setFullYear(year);
        currDate.setMonth(month-1);
        currDate.setDate(day);
        currDate.setHours(options.h);
        if(!!this.maxdate){
          currDate.setMinutes(0);
          currDate.setSeconds(0);
          let currDateTime = new Date(currDate).getTime();
          if(currDateTime > maxDateTime){
            return true;
          }
        }
        if(!!this.mindate){
          currDate.setMinutes(59);
          currDate.setSeconds(59);
          let currDateTime = new Date(currDate).getTime();
          if(currDateTime < minDateTime){
            return true;
          }
        }
      }
      if(options.M  != null){
        let currDate =  new Date();
        currDate.setFullYear(year);
        currDate.setMonth(month-1);
        currDate.setDate(day);
        currDate.setHours(hour);
        currDate.setMinutes(options.M);
        currDate.setSeconds(0);
        let currDateTime = new Date(currDate).getTime();
        if(!!this.maxdate && currDateTime > maxDateTime){
          return true;
        }
        if(!!this.mindate && currDateTime < minDateTime){
          return true;
        }
      }
      return false;
    },
    //前一年
    prevYear() {
      this.year--;
      if(this.type == "week"){
        this.weekData = getWeekData(this.year);
      }
    },
    //后一年
    nextYear() {
      this.year++;
      if(this.type == "week"){
        this.weekData = getWeekData(this.year);
      }
    },
    //前一月
    prevMonth() {
      let {
        year,
        month
      } = this;
      if (month > 1) {
        this.month--;
      } else {
        this.year--;
        this.month = 12;
      }
    },
    //后一月
    nextMonth() {
      let {
        year,
        month
      } = this;
      if (month < 12) {
        this.month++;
      } else {
        this.year++;
        this.month = 1;
      }
    },
    prevDay(){
        this.addDate(-1);
    },
    nextDay(){
        this.addDate(1);
    },
    prevHour(){
        this.addHours(-1);
    },
    nextHour(){
        this.addHours(1);
    },
    //立即关闭日期面板
    immEndChoice() {
      this.isMouseOver = true;
      this.dayPanelIsShow = false;
      this.monthPanelIsShow = false;
      this.weekPanelIsShow = false;
      this.hourPanelIsShow = false;
      this.minutePanelIsShow = false;
    },
    //点击面板
    panelClick: function() {

    },
    //日期加减
    addDate:function(days){
       let date = this.year + "-" + this.month + "-" + this.day;
       let d = new Date(date.replace(/-/g, "/"));
       d.setDate(d.getDate()+days);
       this.year = d.getFullYear();
       this.month = d.getMonth()+1;
       this.day = d.getDate();
     },
     //时间加减
     addHours:function(hour){
        let date = this.year + "-" + this.month + "-" + this.day + " " + this.hour + ":" + this.minute + ":" + this.second;
        let d = new Date(date.replace(/-/g, "/"));
        d.setHours(d.getHours()+hour);
        this.year = d.getFullYear();
        this.month = d.getMonth()+1;
        this.day = d.getDate();
        this.hour = d.getHours();
    },
    //清除日期
    clearData: function() {
      this.$emit('input', "");
      //this.iconClass = "el-icon-date";
    },
    //重置日期
    resetData: function() {
      if (!!this.value) {
        var dateObj = new Date(this.value.replace(/-/g, "/"))
        this.year = dateObj.getFullYear();
        this.month = dateObj.getMonth()+1;
        this.day = dateObj.getDate();
        this.hour = dateObj.getHours();
        this.minute = dateObj.getMinutes();
        this.second = dateObj.getSeconds();
      }
    },
    //日期时间格式化输出
    dateTimeFormat:function(y, M, d, h, m, s, fm) {
      if (!fm) {
        fm = 'yyyy-MM-dd hh:mm:ss';
      }
      M = ('0' + (parseInt(M))).slice(-2);
      d = ('0' + d).slice(-2);
      h = ('0' + h).slice(-2);
      m = ('0' + m).slice(-2);
      s = ('0' + s).slice(-2);
      return fm.replace('yyyy', y)
        .replace('YYYY', y)
        .replace('MM', M)
        .replace('dd', d)
        .replace('hh', h)
        .replace('mm', m)
        .replace('ss', s);
    },
    changeIconClose (type) {
      if(!!this.value && 'in'){
        this.iconState = true;
        this.iconClass = "el-icon-close";
      }
    },
    changeIconDate (type) {
      this.iconState = false;
      this.iconClass = "el-icon-date";
    }
  }
}

Array.prototype.includes = function(item) {
  var isExsit = false;
  var Len = this.length;
  for (var i = 0; i < Len; i++) {
    var arrItem = this[i];
    if (item == arrItem) {
      isExsit = true;
      break;
    }
  }
  return isExsit;
}

function getDayArr(l1, l2) {
  var mainArr = new Array();
  for (var i = 0; i < l1; i++) {
    var subArr = new Array();
    for (var j = 0; j < l2; j++) {
      subArr.push('');
    }
    mainArr.push(subArr);
  }
  return mainArr;
}

function formatDig(num){
    return num>9?''+num:'0'+num;
}
function formatDate(mill){
    var y=new Date(mill);
    let raws= [
        y.getFullYear(),
        formatDig(y.getMonth()+1),
        formatDig(y.getDate()),
        //y.getDay()||7
    ];
    let format=['-','-',''];
    return String.raw({raw:raws},...format);
}
function* createWeeks(year){
    const ONE_DAY=24*3600*1000;
    let start=new Date(year,0,1),
           end=new Date(year,11,31);
    let firstDay=start.getDay()|| 7,
            lastDay=end.getDay()||7;
    let startTime=+start,
            endTime=startTime+(7-firstDay)*ONE_DAY,
            _endTime=end-(7-lastDay)*ONE_DAY;
    if(firstDay == 1){//以当年第一个周一为第一周
      yield [startTime,endTime];
    }
    startTime=endTime+ONE_DAY;
    endTime=endTime+7*ONE_DAY;
    while(endTime<_endTime){
        yield [startTime,endTime];
        startTime=endTime+ONE_DAY;
        endTime=endTime+7*ONE_DAY;
    }
    //yield [startTime,+end];
    yield [startTime,endTime];//最后一周为当年的最后一个周一
}

function getWeekData(year){
  let index=1;
  let list = createWeeks(year);
  let weekList = [];
  for(let i of list){
      let start=i[0],
              end=i[1];
      let weekIndex = formatDig(index++);
      let weekStart = formatDate(start);
      let weekEnd = formatDate(end);
      let weekShortText = "第"+weekIndex+"周 "+weekStart;
      let weekText = "第"+weekIndex+"周 "+weekStart+" ~ "+weekEnd;
      let weekStartDate = start;
      let weekEndDate = end;
      weekList.push({
        weekIndex,
        weekStart,
        weekEnd,
        weekShortText,
        weekText,
        weekStartDate,
        weekEndDate
      });
  }
  return weekList;
}





</script>

<style scoped>
@-webkit-keyframes vueDatePicker {
    0% {
        opacity: 0;
        -webkit-transform: translate(-50%, -50%) scale(0);
        transform: translate(-50%, -50%) scale(0);
    }
    100% {
        opacity: 1;
        -webkit-transform: translate(0) scale(1);
        transform: translate(0) scale(1);
    }
}

@keyframes vueDatePicker {
    0% {
        opacity: 0;
        -webkit-transform: translate(-50%, -50%) scale(0);
        transform: translate(-50%, -50%) scale(0);
    }
    100% {
        opacity: 1;
        -webkit-transform: translate(0) scale(1);
        transform: translate(0) scale(1);
    }
}

.vue-datepicker {
    position: relative;
    margin: 0;
    padding: 0;
    font-size: 12px;
    color: #666;
    width: 160px;
    display: inline-block;
}

.vue-datepicker * {
    margin: 0;
    padding: 0;
}

.vue-datepicker input {
    display: block;
    width: 245px;

    padding-left: 6px;

    outline: none;
}

.vue-datepicker .vue-datepicker-panels {
    position: absolute;
    z-index: 99999;
    left: 0;
    background-color: white;
    margin-top: 4px;
    width: 252px;
    border: 1px solid #DDD;
    -webkit-box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.18);
    box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.18);
    padding: 10px 6px;
    -webkit-animation: vueDatePicker 0.1s ease-out;
    animation: vueDatePicker 0.1s ease-out;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-month {
    padding-bottom: 4px;
    height: 35px;
    line-height: 35px;
    overflow: hidden;
    text-align: center;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-month a {
    float: left;
    display: block;
    width: 35px;
    cursor: pointer;
    color: #999;
    font-size: 12px;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-month a:hover {
    background-color: #F5F6F7;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-month span {
    float: left;
    display: block;
    width: 168px;
    font-size: 14px;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-month span.vue-datepicker-btn:hover {
    background-color: #F5F6F7;
    cursor: pointer;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 {
    width: 228px;
    margin-left: 5px;
    border-collapse: collapse;
    text-align: center;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb thead,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 thead {
    height: 35px;
    line-height: 35px;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb thead tr,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 thead tr {
    border: none;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb thead tr th,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 thead tr th {
    width: 35px;
    font-size: 12px;
    border: none;
    font-weight: normal;
    color:#8391a5;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr {

}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td {
    height: 31.5px;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed {
    cursor: pointer;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed span,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed span {
    display: block;
    height: 31.7px;
    line-height: 31.7px;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed:hover,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed:hover {
    background-color: #e4e8f1;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed.z-on span,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed.z-on span {
    color: #FFF;
    background-color: #007fff;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed.z-invalid,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed.z-invalid {
    cursor: default;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed.z-invalid span,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed.z-invalid span {
    color: #ccc;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb tbody tr td.z-existed.z-invalid:hover,
.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed.z-invalid:hover {
    background-color: transparent;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr {

}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td {
    height: 31.5px;
    font-size: 12px;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed {
    cursor: pointer;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-tb2 tbody tr td.z-existed span {
    display: block;
    height: 39.75px;
    line-height: 39.75px;
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-time-panel{
  height: 128px;
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-time{
  padding-top: 4px;
  height: 35px;
  line-height: 35px;
  overflow: hidden;
  text-align: center;
  background-color: #F5F6F7;
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-time a{
  display: inline-block;
    width: 50%;
    height: 35px;
    line-height: 27px;
    float: left;
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-timeUnit{
  width: 33.3%;
  float: left;
  height: 100%;
  text-align: center;
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-timeUnit .timeBtn{
  height: 40%;
  width: 100%;
  position: relative;
  cursor: pointer;
}

.vue-datepicker .vue-datepicker-panel .vue-datepicker-timeUnit .timeBtn i{
  position: absolute;
  top:50%;
  left: 50%;
  transform: translate(-50%,-50%);
  width: 100%;
  -webkit-transform: translate(-50%,-50%);
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-timeUnit .numPanel{
  height: 20%;
  width: 100%;
  position: relative;
}
.vue-datepicker .vue-datepicker-panel .vue-datepicker-timeUnit .numPanel span{
  position: absolute;
  top:50%;
  left: 50%;
  transform: translate(-50%,-50%);
  -webkit-transform: translate(-50%,-50%);
}
.closeIcon{
  position: absolute;
  width: 20px;
  height: 30px;
  line-height: 30px;
  color: black;
  top: 0;
  right: 0;
  text-align: center;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
}
.dateInputPanel {
  width: 25px;
  text-align: center;
  padding: 0;
  border: 0px solid red;
}
.vue-datepicker input{
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    background-color: #fff;
    background-image: none;
    border-left: none;
    border-right: none;
    border-top: none;
    border-bottom: 1px solid #ededed;
    box-sizing: border-box;
    color: #808080;
    display: block;
    font-size: inherit;
    height: 30px;
    line-height: 1;
    outline: 0;
    padding: 3px 10px;
    transition: border-color 0.2s cubic-bezier(0.645, 0.045, 0.355, 1);
    padding-right: 35px;
    background-image: none;
    width: 100%;
    border-bottom-width:0px;
}
.zm_data_ic{
  position:absolute;
  width:35px;
  height:30px;
  line-height: 30px;
  right:0;
  top:0;
  text-align:center;
  color:#808080;
  transition:all 0.3s;
}
.week-content{
  height: 300px;
  overflow-y: auto;
  text-align: center;
}
.week-item{
  height: 30px;
  line-height: 30px;
  cursor: pointer;
}
.week-item.select{
  color: #FFF;
  background-color: #007fff;
}
.week-item:hover{
  background-color: #F5F6F7;
  color: #666;
}
.week-input,.month-input,.day-input,.hour-input,.minute-input{
  position: absolute;
  top:0;
  left:0;
  width: 100%;
  height: 30px;
  line-height: 30px;
  background-color: white;
  cursor: text;
  text-align: center;
  color:#B4BCCC;
}

</style>
