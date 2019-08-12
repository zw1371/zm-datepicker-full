# zm-datepicker-full
    vue日期控件(pc端)
    ![GitHub Logo](https://images2018.cnblogs.com/blog/590271/201803/590271-20180323182505819-79738066.png)
    (具体效果图见：https://www.cnblogs.com/mrzhu/p/8632396.html)

# 使用

```vue
<zm-datepicker 
    mindate="2018-1-26 12:12:12" 
    maxdate="2018-3-26 12:12:12" 
    type="month" 
    v-model="date">
</zm-datepicker>
```

```javascript
{
    data: () => {
      return {
        date: ''
      }
    }
}
```

# 属性说明
  * placeholder 占位文本
  * mindate 可选择最小日期
  * maxdate 可选择最大日期
  * type 选择器类型
    * month:选取到月份
    * week:选取到周
    * day:选取到天
    * hour:选取到小时
    * minute:选取到分钟(默认)

# 事件说明
  * 事件名 select-time 
  * 返回值 
    * type 当前日历类型
    * data 用户选取的数据(具体格式见下边)

# select-time事件返回值data字段具体说明
    type是month
    > data数据格式类型：
      {
        date,//yyyy-MM格式日期
        day,//yyyy-MM-dd格式日期
        time//yyyy-MM-dd hh:mm:ss格式日期
      }

    type是week
    > data数据格式类型：
      {
        weekIndex:w,//当前周的索引
        monday:s,//当前选择的周的周一的日期 格式为yyyy-MM-dd
        sunday:e,//当前选择周的周日的日期 格式为yyyy-MM-dd
        time//时间 格式为yyyy-MM-dd hh:mm:ss
      } 

    type是day
    > data数据格式类型：
      {
        date,//yyyy-MM-dd格式数据
        day,//yyyy-MM-dd格式数据
        time//yyyy-MM-dd hh:mm:ss格式数据
      } 

    type是hour
    > data数据格式类型：
      {    
        date,//yyyy-MM-dd hh格式数据
        day,//yyyy-MM-dd格式数据
        time//yyyy-MM-dd hh:mm:ss格式数据
      }

    type是minute
    > data数据格式类型：
      {
        date,//yyyy-MM-dd hh:mm格式数据
        day,//yyyy-MM-dd格式数据
        time//yyyy-MM-dd hh:mm:ss格式数据
      }