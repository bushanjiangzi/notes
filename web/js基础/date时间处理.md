## new Date()
  - new Date() - Fri Mar 19 2021 13:47:10 GMT+0800 (中国标准时间)

  - getFullYear() - 从 Date 对象以四位数字返回年份

  - getMonth() - 从 Date 对象返回月份 (0 ~ 11)

  - getDate() - 从 Date 对象返回一个月中的某一天 (1 ~ 31)

  - getDay() - 从 Date 对象返回一周中的某一天 (0 ~ 6)

  - getHours() - 返回 Date 对象的小时 (0 ~ 23)

  - getMinutes() - 返回 Date 对象的分钟 (0 ~ 59)

  - getSeconds() - 返回 Date 对象的秒数 (0 ~ 59)

  - getMilliseconds() - 返回 Date 对象的毫秒(0 ~ 999)

  - getTime() - 返回 1970 年 1 月 1 日至今的毫秒数

  - getTimezoneOffset() - 返回本地时间与格林威治标准时间 (GMT) 的分钟差

  - Date.parse - 返回1970年1月1日午夜到指定日期（字符串）的毫秒数,如：`Date.parse('2021-03-19:13:55:20')` // 1616133320000

  - toLocaleString() - 根据本地时间格式，把 Date 对象转换为字符串；`(new Date()).toLocaleString()` // "2021/3/19下午2:06:23"

  - toLocaleTimeString() - 根据本地时间格式，把 Date 对象的时间部分转换为字符串；`(new Date()).toLocaleTimeString()` // "下午2:09:03"

  - toLocaleDateString() - 根据本地时间格式，把 Date 对象的日期部分转换为字符串；`(new Date()).toLocaleDateString()` // "2021/3/19"

  - 获取当前时间戳 - `Date.now()`或`new Date().getTime()`，Date.now()相当于在Date原型上获取时间戳，而new Date()实例化性能消耗比Date.now()大

  - 获取ISO 标准时间 - `new Date().toISOString()`，返回格式："2021-03-19T06:13:55.143Z"

  - 获取中国本地标准时间 - 因为时区问题会相差8小时，`(new Date(+new Date() + 8 * 3600 * 1000)).toISOString()`，返回格式："2021-03-19T14:16:59.448Z"

## 时间转化处理方法

  - 将中国标准转为YY-MM-DD hh:mm:ss
  ```
  changeToYMDhms() {
    let date = new Date()
    let y = date.getFullYear()
    let m = date.getMonth() + 1
    m = m < 10 ? '0' + m : m
    let d = date.getDate()
    d = d < 10 ? '0' + d : d
    let h = date.getHours()
    let minute = date.getMinutes()
    minute = minute < 10 ? '0' + minute : minute
    let second = date.getSeconds()
    second = second < 10 ? '0' + second : second
    let time = y + '-' + m + '-' + d + ' ' + h + ':' + minute + ':' + second
    console.log(time)
    return time
  }
  ```

  - 将中国标准时间转为HH:mm:ss
  ```
  changeToHms() {
    let date = new Date()
    let h = date.getHours()
    let minute = date.getMinutes()
    minute = minute < 10 ? '0' + minute : minute
    let second = date.getSeconds()
    second = second < 10 ? '0' + second : second
    return h + ':' + minute + ':' + second
  }
  ```

  - 将计时器的时间转为时分秒
  ```
  timesToHHMMSS(times) {
    let h = Math.floor(times / 3600)
    let m = Math.floor((times / 60) % 60)
    let s = times % 60
    h = h > 10 ? h : '0' + h
    m = m > 10 ? m : '0' + m
    s = s > 10 ? s : '0' + s
    return `${h}:${m}:${s}`
  }
  ```