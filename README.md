# 这是一些javascript的常用小轮子

#时间相关的一些方法

/**
 * 计算时间差
 * @param star 必须是时间戳
 * @param diff 时间相差天数
 * @returns {*} 日期字符串
 */
```javascript
// console.log(timeDifference(new Date(), -20))
function timeDifference(star, diff) {
    //star必须是时间戳
    //diff时间相差天数
    var starTime = star;
    if (star.length == 10) {
        starTime = star * 1000;
    }
    var now = new Date(starTime); //当前日期
    now.setDate(now.getDate() + diff);
    return formatDate(now);

    //格式化时间
    function formatDate(date) {
        var myyear = date.getFullYear();
        var mymonth = date.getMonth() + 1;
        var myweekday = date.getDate();

        if (mymonth < 10) {
            mymonth = "0" + mymonth;
        }
        if (myweekday < 10) {
            myweekday = "0" + myweekday;
        }
        return (myyear + "-" + mymonth + "-" + myweekday);
    }
}
```
/**
 * 按照指定格式将日期转换成字符串
 * @param format 日期格式
 * @returns {*} 日期字符串
 */
```javascript
Date.prototype.format = function (format) {
    var o = {
        "M+": this.getMonth() + 1, //month
        "d+": this.getDate(),    //day
        "h+": this.getHours(),   //hour
        "m+": this.getMinutes(), //minute
        "s+": this.getSeconds(), //second
        "q+": Math.floor((this.getMonth() + 3) / 3),  //quarter
        "S": this.getMilliseconds() //millisecond
    }
    if (/(y+)/.test(format)) format = format.replace(RegExp.$1,
        (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    for (var k in o)if (new RegExp("(" + k + ")").test(format))
        format = format.replace(RegExp.$1,
            RegExp.$1.length == 1 ? o[k] :
                ("00" + o[k]).substr(("" + o[k]).length));
    return format;
}
```
/**
 * 获取一定时间周期的具体时间
 * @param data 详情请看if判断
 * @returns {*} 日期字符串
 */
```javascript
 //console.log(getDataDay('yesterday'));
function getDataDay(data) {
   /**         * 时间日期类
         */
        var now = new Date(); //当前日期
        var nowDayOfWeek = now.getDay(); //今天本周的第几天
        var nowDay = now.getDate(); //当前日
        var nowMonth = now.getMonth(); //当前月
        var nowYear = now.getYear(); //当前年
        nowYear += (nowYear < 2000) ? 1900 : 0; //
        if (data == 'today') {
            var yesterday = new Date(nowYear, nowMonth, nowDay); //今天
            return formatDate(yesterday);
        } else if (data == 'yesterday') {
            var yesterday = new Date(nowYear, nowMonth, nowDay - 1); //昨天
            return formatDate(yesterday);
        } else if (data == 'weekStart') {
            //获取本周第一天日期
            var weekStart = new Date(nowYear, nowMonth, nowDay + (1 - nowDayOfWeek));
            return formatDate(weekStart);
        } else if (data == 'weekEnd') {
            //获取本周最后一天日期
            var weekStart = new Date(nowYear, nowMonth, nowDay + (7 - nowDayOfWeek));
            return formatDate(weekStart);
        } else if (data == 'lastWeekStart') {
            //获得上周第一天日期
            var lastWeekStart = new Date(nowYear, nowMonth, nowDay + (-6 - nowDayOfWeek));
            return formatDate(lastWeekStart);
        } else if (data == 'lastWeekEnd') {
            //获得上周最后一天日期
            var lastWeekEnd = new Date(nowYear, nowMonth, nowDay - nowDayOfWeek);
            return formatDate(lastWeekEnd);
        } else if (data == 'monthStart') {
            //本月第一天
            var monthStartDate = new Date(nowYear, nowMonth, 1);
            return formatDate(monthStartDate);
        } else if (data == 'monthEnd') {
            //本月最后一天
            var monthStartDate = new Date(nowYear, nowMonth + 1, -1);
            return formatDate(monthStartDate);
        } else if (data == 'lastMonthStart') {
            //上月第一天
            var monthStartDate = new Date(nowYear, (nowMonth - 1), 1);
            return formatDate(monthStartDate);
        } else if (data == 'LastMonthEnd') {
            //上月最后一天
            var monthStartDate = new Date(nowYear, nowMonth, -1);
            return formatDate(monthStartDate);
        }
    //格式化时间
    function formatDate(date) {
        var myyear = date.getFullYear();
        var mymonth = date.getMonth() + 1;
        var myweekday = date.getDate();

        if (mymonth < 10) {
            mymonth = "0" + mymonth;
        }
        if (myweekday < 10) {
            myweekday = "0" + myweekday;
        }
        return (myyear + "/" + mymonth + "/" + myweekday);
    }
}
```


#Cookie操作
```javascript
function getCookie(c_name) {
    if (document.cookie.length > 0) {
        var c_start = document.cookie.indexOf(c_name + "=")
        if (c_start != -1) {
            c_start = c_start + c_name.length + 1;
            var c_end = document.cookie.indexOf(";", c_start);
            if (c_end == -1) c_end = document.cookie.length;
            return unescape(document.cookie.substring(c_start, c_end));
        }
    }
    return "";
}

function setCookie(c_name, value, expiredays) {
    if (!expiredays) {
        expiredays = 100;
    }
    var exdate = new Date();
    exdate.setDate(exdate.getDate() + expiredays);
    document.cookie = c_name + "=" + escape(value) + ((expiredays == null) ? "" : ";expires=" + exdate.toGMTString());
}
```
# 获取一个唯一ID
JavaScript生成GUID的算法

全局唯一标识符（GUID，Globally Unique Identifier）也称作 UUID(Universally Unique IDentifier) 。

GUID是一种由算法生成的二进制长度为128位的数字标识符。GUID 的格式为“xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx”，其中的 x 是 0-9 或 a-f 范围内的一个32位十六进制数。在理想情况下，任何计算机和计算机集群都不会生成两个相同的GUID。

GUID 的总数达到了2^128（3.4×10^38）个，所以随机生成两个相同GUID的可能性非常小，但并不为0。GUID一词有时也专指微软对UUID标准的实现。
```javascript
//算法1
function uuid() {
    var s = [];
    var hexDigits = "0123456789abcdef";
    for (var i = 0; i < 36; i++) {
        s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
    }
    s[14] = "4";  // bits 12-15 of the time_hi_and_version field to 0010
    s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);  // bits 6-7 of the clock_seq_hi_and_reserved to 01
    s[8] = s[13] = s[18] = s[23] = "-";
 
    var uuid = s.join("");
    return uuid;
}
//算法2
function guid() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = Math.random()*16|0, v = c == 'x' ? r : (r&0x3|0x8);
        return v.toString(16);
    });
}
//算法3
function guid() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = Math.random()*16|0, v = c == 'x' ? r : (r&0x3|0x8);
        return v.toString(16);
    });
}
//算法3
function guid() {
    function S4() {
       return (((1+Math.random())*0x10000)|0).toString(16).substring(1);
    }
    return (S4()+S4()+"-"+S4()+"-"+S4()+"-"+S4()+"-"+S4()+S4()+S4());
}
//算法4
function uuid(len, radix) {
    var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'.split('');
    var uuid = [], i;
    radix = radix || chars.length;
 
    if (len) {
      // Compact form
      for (i = 0; i < len; i++) uuid[i] = chars[0 | Math.random()*radix];
    } else {
      // rfc4122, version 4 form
      var r;
 
      // rfc4122 requires these characters
      uuid[8] = uuid[13] = uuid[18] = uuid[23] = '-';
      uuid[14] = '4';
 
      // Fill in random data.  At i==19 set the high bits of clock sequence as
      // per rfc4122, sec. 4.1.5
      for (i = 0; i < 36; i++) {
        if (!uuid[i]) {
          r = 0 | Math.random()*16;
          uuid[i] = chars[(i == 19) ? (r & 0x3) | 0x8 : r];
        }
      }
    }
 
    return uuid.join('');
}
//这个可以指定长度和基数。比如：
// 8 character ID (base=2)
uuid(8, 2)  //  "01001010"
// 8 character ID (base=10)
uuid(8, 10) // "47473046"
// 8 character ID (base=16)
uuid(8, 16) // "098F4D35"
 

```
