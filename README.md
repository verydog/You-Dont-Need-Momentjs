Moment.js是一个很棒的时间和日期库，具有许多牛X的方法，平时`npm install`了之后就是干，但是如果您的Web应用程序对性能上有很高的要求，可能会由于其复杂的API和大小会导致巨大的性能上的比不要的开销。
![](https://user-gold-cdn.xitu.io/2018/9/18/165eab3aab9f7ed0?w=496&h=85&f=png&s=31449)

## Moment存在的一些问题
* 它高度基于OOP API，这使得它无法使用Webpack 2 新引入的`Tree-shaking`代码优化技术
* 由于OOP API还有非纯函数，这会导致一些bug https://github.com/moment/moment/blob/develop/src/test/moment/add_subtract.js#L244-L286
* 如果您没有使用时区，而只使用了moment.js中的一些简单函数，这会导致你的应用程序被引入了很多没使用的方法，这是极其浪费性能和内存的。 在这里推荐使用`dayjs`, `dayjs`有一个较小的核心，并且具有非常相似的API，因此很容易从moment平滑过渡到day.js。date-fns可以使用`Tree-shaking`代码优化技术和其他good api，因此它很适合与React，Sinon.js和webpack等好基友一起使用。

## 简单比较
| 名字                                     | 大小(gzip)                        | 支持Tree-shaking | 名气 | api方法数 | 模式    | 时区支持      | 支持的语言数 |
| ---------------------------------------- | --------------------------------- | ------------ | ---------- | ---------------- | ---------- | --------------------- | ------ |
| [Moment.js](https://momentjs.com/)       | 329K(69.6K)                       | No           | 38k        | 高             | OO         | 非常好(moment-timezone) | 123    |
| [date-fns](https://date-fns.org)         | 78.4k(13.4k) without tree-shaking | Yes          | 13k        | 高             | Functional | 还不支持               | 32     |
| [dayjs](https://github.com/iamkun/dayjs) | 6.5k(2.6k) without plugins        | No           | 14k        | 中           | OO         | 还不支持               | 23     |

## 程序员吐槽

> [把了moment.js换成了date-fns - 构建输出减少了40％](https://github.com/oysterprotocol/webnode/pull/116)

> &mdash;<cite>Jared Farago from [webnode](https://github.com/oysterprotocol/webnode/pull/116) project.</cite>

> [请使用JavaScript原生时间对象和数组。 如果您因为某种原因想要用Moment.js，那么这是一个很好的库。 差不多。](https://twitter.com/dan_abramov/status/805030922785525760)

> &mdash;<cite>Dan Abramov, Author of [Redux](https://github.com/reduxjs/redux) and co-author of [Create React App](https://github.com/facebook/create-react-app). Building tools for humans.</cite>

> [我强烈建议在Moment.js上使用date-fns，它有一个更好的API，你只能包含你需要的部分！](https://twitter.com/silvenon/status/804946772690923520)

> &mdash;<cite>Matija Marohnić, a design-savvy frontend developer from Croatia.</cite>

> [就在昨天，在项目中将momentjs换了，我们项目体积减少了很多😱](https://twitter.com/gribnoysup/status/805061630752997377)

> &mdash;<cite>Sergey Petushkov, a javaScript developer from Moscow, Russia • Currently in Berlin, Germany.</cite>

## ESLint插件
<p align="center">
  <a href="https://www.npmjs.com/package/eslint-plugin-you-dont-need-momentjs">
    <img src="https://img.shields.io/npm/v/eslint-plugin-you-dont-need-momentjs.svg?style=flat-square"
      alt="NPM Version">
  </a>
  <a href="https://www.npmjs.org/package/eslint-plugin-you-dont-need-momentjs">
    <img src="https://img.shields.io/npm/dm/eslint-plugin-you-dont-need-momentjs.svg?style=flat-square?style=flat-square"
      alt="Downloads">
  </a>
  <a href="https://travis-ci.org/you-dont-need/You-Dont-Need-Momentjs">
    <img src="https://img.shields.io/travis/you-dont-need/You-Dont-Need-Momentjs/master.svg?style=flat-square"
      alt="Build Status">
  </a>
  <a href="https://coveralls.io/github/you-dont-need/You-Dont-Need-Momentjs?branch=master">
    <img src="https://img.shields.io/coveralls/you-dont-need/You-Dont-Need-Momentjs/master.svg?style=flat-square"
      alt="Coverage Status" />
  </a>
  <a href="https://david-dm.org/you-dont-need/You-Dont-Need-Momentjs">
    <img src="https://img.shields.io/david/you-dont-need/You-Dont-Need-Momentjs.svg?style=flat-square"
         alt="Dependency Status">
  </a>
</p>

如果你正在使用 [ESLint](http://eslint.org/), 你可以安装一个插件
[plugin](http://eslint.org/docs/user-guide/configuring#using-the-configuration-from-a-plugin) 来帮助你识别代码库中你没有（可能不需要）Moment.js的地方。

安装这个插件...

```sh
npm install --save-dev eslint-plugin-you-dont-need-momentjs
```

...然后更新你的配置

```js
"extends" : ["plugin:you-dont-need-momentjs/recommended"],
```

## 使用对比

⚠️_注意提供的date-fns示例适用于[v2]（https://date-fns.org/v2.0.0-alpha.16/docs/Getting-Started），该版本现已发布。 [请参阅v1 docs]（https://date-fns.org/docs/Getting-Started）了解当前版本._


## 解析

### 字符串+日期格式化

返回从字符串中解析的日期.

```js
// Moment.js
moment('12-25-1995', 'MM-DD-YYYY');
// => "1995-12-24T13:00:00.000Z"

// date-fns
import parse from 'date-fns/parse';
parse('12-25-1995', 'MM-dd-yyyy', new Date());
// => "1995-12-24T13:00:00.000Z"

// dayjs
dayjs('12-25-1995');
// => "1995-12-24T13:00:00.000Z"
```

<br>



### 字符串+时间格式化

返回从字符串中解析的时间日期.

```js
// Moment.js
moment('2010-10-20 4:30', 'YYYY-MM-DD HH:mm');
// => "2010-10-19T17:30:00.000Z"

// date-fns
import parse from 'date-fns/parse';
parse('2010-10-20 4:30', 'yyyy-MM-dd H:mm', new Date());
// => "2010-10-19T17:30:00.000Z"

// dayjs❌不支持自定义格式解析
```

<br>


### 字符串+本地格式化  

返回从字符串中解析的本地化时间日期.

```js
// Moment.js
moment('2012 mars', 'YYYY MMM', 'fr');
// => "2012-02-29T13:00:00.000Z"

// date-fns
import parse from 'date-fns/parse';
import fr from 'date-fns/locale/fr';
parse('2012 mars', 'yyyy MMMM', new Date(), { locale: fr });
// => "2012-02-29T13:00:00.000Z"

// dayjs❌不支持自定义格式解析
```


## 取值 + 赋值

### 毫秒/秒/分/时

获取 `毫秒 / 秒 / 分 / 时`。

```js
// Moment.js
moment().seconds();
// => 49
moment().hours();
// => 19

// 原生
new Date().getSeconds();
// => 49
new Date().getHours();
// => 19

// date-fns
import getSeconds from 'date-fns/getSeconds';
import getHours from 'date-fns/getHours';
getSeconds(new Date());
// => 49
getHours(new Date());
// => 19

// dayjs
dayjs().second();
// => 49
dayjs().hour();
// => 19
```

设置 `毫秒 / 秒 / 分 / 时`。

```js
// Moment.js
moment().seconds(30);
// => "2018-09-09T09:12:30.695Z"
moment().hours(13);
// => "2018-09-09T03:12:49.695Z"

// 原生
new Date(new Date().setSeconds(30));
// => "2018-09-09T09:12:30.695Z"
new Date(new Date().setHours(13));
// => "2018-09-09T03:12:49.695Z"

// date-fns
import setSeconds from 'date-fns/setSeconds';
import setHours from 'date-fns/setHours';
setSeconds(new Date(), 30);
// => "2018-09-09T09:12:30.695Z"
setHours(new Date(), 13);
// => "2018-09-09T03:12:49.695Z"

// dayjs
dayjs().set('second', 30);
// => "2018-09-09T09:12:30.695Z"
dayjs().set('hour', 13);
// => "2018-09-09T03:12:49.695Z"
```

<br>



### 月份日期

设置&获取月份。

```js
// Moment.js
moment().date();
// => 9
moment().date(4);
// => "2018-09-04T09:12:49.695Z"

// 原生
new Date().getDate();
// => 9
new Date().setDate(4);
// => "2018-09-04T09:12:49.695Z"

// date-fns
import getDate from 'date-fns/getDate';
import setDate from 'date-fns/setDate';
getDate(new Date());
// => 9
setDate(new Date(), 4);
// => "2018-09-04T09:12:49.695Z"

// dayjs
dayjs().date();
// => 9
dayjs().set('date', 4);
// => "2018-09-04T09:12:49.6
```

<br>


### 星期几

设置&获取星期。

```js
// Moment.js
moment().day();
// => 0 (Sunday)
moment().day(-14);
// => "2018-08-26T09:12:49.695Z"

// 原生
new Date().getDay();
// => 0 (Sunday)
new Date().setDate(new Date().getDate() - 14);
// => "2018-08-26T09:12:49.695Z"

// date-fns
import getDay from 'date-fns/getDay';
import setDay from 'date-fns/setDay';
getDay(new Date());
// => 0 (Sunday)
setDay(new Date(), -14);
// => "2018-08-26T09:12:49.695Z"

// dayjs
dayjs().day();
// => 0 (Sunday)
dayjs().set('day', -14);
// => "2018-08-26T09:12:49.695Z"
```

<br>



### 一年的某一天

设置&获取一年的某一天。

```js
// Moment.js
moment().dayOfYear();
// => 252
moment().dayOfYear(256);
// => "2018-09-13T09:12:49.695Z"

// date-fns
import getDayOfYear from 'date-fns/getDayOfYear';
import setDayOfYear from 'date-fns/setDayOfYear';
getDayOfYear(new Date());
// => 252
setDayOfYear(new Date(), 256);
// => "2018-09-13T09:12:49.695Z"

// dayjs ❌  不支持
```
<br>


### 一年的某一周

设置&获取一年的某一周。

```js
// Moment.js
moment().week();
// => 37
moment().week(24);
// => "2018-06-10T09:12:49.695Z"

// date-fns
import getWeek from 'date-fns/getWeek';
import setWeek from 'date-fns/setWeek';
getWeek(new Date());
// => 37
setWeek(new Date(), 24);
// => "2018-06-10T09:12:49.695Z"

// dayjs ⚠️ requires weekOfYear plugin
import weekOfYear from 'dayjs/plugin/weekOfYear';
dayjs.extend(weekOfYear);
dayjs().week();
// => 37
// dayjs ❌  不支持
```
<br>

### 某月有多少天

获取某月有多少天。

```js
// Moment.js
moment('2012-02', 'YYYY-MM').daysInMonth();
// => 29

// date-fns
import getDaysInMonth from 'date-fns/getDaysInMonth';
getDaysInMonth(new Date(2012, 1));
// => 29

// dayjs
dayjs('2012-02').daysInMonth();
// => 29
```
<br>

### 一年有多少周

根据ISO周，获取当年的周数。

```js
// Moment.js
moment().isoWeeksInYear();
// => 52

// date-fns
import getISOWeeksInYear from 'date-fns/getISOWeeksInYear';
getISOWeeksInYear(new Date());
// => 52

// dayjs ❌ does not support weeks in the year
```
<br>


### 获取日期最大值

返回给定日期的最大值。

```js
const array = [
  new Date(2017, 4, 13),
  new Date(2018, 2, 12),
  new Date(2016, 0, 10),
  new Date(2016, 0, 9),
];
// Moment.js
moment.max(array.map(a => moment(a)));
// => "2018-03-11T13:00:00.000Z"

// 原生
new Date(Math.max.apply(null, array)).toISOString();
// => "2018-03-11T13:00:00.000Z"

// date-fns
import max from 'date-fns/max';
max(array);
// => "2018-03-11T13:00:00.000Z"

// dayjs ❌  不支持
```


<br>

### 获取日期最小值

返回给定日期的最小值。

```js
const array = [
  new Date(2017, 4, 13),
  new Date(2018, 2, 12),
  new Date(2016, 0, 10),
  new Date(2016, 0, 9),
];
// Moment.js
moment.min(array.map(a => moment(a)));
// => "2016-01-08T13:00:00.000Z"

// 原生
new Date(Math.min.apply(null, array)).toISOString();
// => "2016-01-08T13:00:00.000Z"

// date-fns
import min from 'date-fns/min';
min(array);
// => "2016-01-08T13:00:00.000Z"

// dayjs ❌  不支持
```
<br>

## 操作比较

### 添加天数

将指定的天数添加到给定日期。

```js
// Moment.js
moment().add(7, 'days');
// => "2018-09-16T09:12:49.695Z"

// date-fns
import addDays from 'date-fns/addDays';
addDays(new Date(), 7);
// => "2018-09-16T09:12:49.695Z"

// dayjs
dayjs().add(7, 'day');
// => "2018-09-16T09:12:49.695Z"
```
<br>



### 减去天数

从给定日期减去指定的天数。

```js
// Moment.js
moment().subtract(7, 'days');
// => "2018-09-02T09:12:49.695Z"

// date-fns
import subDays from 'date-fns/subDays';
subDays(new Date(), 7);
// => "2018-09-02T09:12:49.695Z"

// dayjs
dayjs().subtract(7, 'day');
// => "2018-09-02T09:12:49.695Z"
```
<br>



### 获月初时间

获取这个月初时间。

```js
// Moment.js
moment().startOf('month');
// => "2018-08-31T14:00:00.000Z"

// date-fns
import startOfMonth from 'date-fns/startOfMonth';
startOfMonth(new Date());
// => "2018-08-31T14:00:00.000Z"

// dayjs
dayjs().startOf('month');
// => "2018-08-31T14:00:00.000Z"
```

<br>


### 获取今天结束的时间

获取今天结束的时间。

```js
// Moment.js
moment().endOf('day');
// => "2018-09-09T13:59:59.999Z"

// date-fns
import endOfDay from 'date-fns/endOfDay';
endOfDay(new Date());
// => "2018-09-09T13:59:59.999Z"

// dayjs
dayjs().endOf('day');
// => "2018-09-09T13:59:59.999Z"
```

<br>



## 显示

### 格式化

用给定的格式格式化给定的字符串。

```js
// Moment.js
moment().format('dddd, MMMM Do YYYY, h:mm:ss A');
// => "Sunday, September 9th 2018, 7:12:49 PM"
moment().format('ddd, hA');
// => "Sun, 7PM"

// date-fns
import format from 'date-fns/format';
format(new Date(), 'eeee, MMMM do YYYY, h:mm:ss aa');
// => "Sunday, September 9th 2018, 7:12:49 PM"
format(new Date(), 'eee, ha');
// => "Sun, 7PM"

// dayjs
dayjs().format('dddd, MMMM D YYYY, h:mm:ss A');
// => "Sunday, September 9 2018, 7:12:49 PM" ⚠️  not support 9th
dayjs().format('ddd, hA');
// => "Sun, 7PM"
```

<br>




### 获取到现在的年限

获取到现在的年限

```js
// Moment.js
moment(1536484369695).fromNow();
// => "4 days ago"

// date-fns
import formatDistance from 'date-fns/formatDistance';
formatDistance(new Date(1536484369695), new Date(), { addSuffix: true });
// => "4 days ago"

// dayjs ⚠️ requires relativeTime plugin
import relativeTime from 'dayjs/plugin/relativeTime';
dayjs.extend(relativeTime);

dayjs(1536484369695).fromNow();
// => "5 days ago" ⚠️  这个插件的舍入方法与moment.js和date-fns不同，请谨慎使用。
```

<br>

### 时差

返回两个时间点的时差。

```js
// Moment.js
moment([2007, 0, 27]).to(moment([2007, 0, 29]));
// => "in 2 days"

// date-fns
import formatDistance from 'date-fns/formatDistance';
formatDistance(new Date(2007, 0, 27), new Date(2007, 0, 29));
// => "2 days"

// dayjs ⚠️ 需要relativeTime插件
import relativeTime from 'dayjs/plugin/relativeTime';
dayjs.extend(relativeTime);
dayjs('2007-01-27').to(dayjs('2007-01-29'));
// => "in 2 days"
```
<br>

### 时差（毫秒）

返回两个时间点的毫秒级时差。

```js
// Moment.js
moment([2007, 0, 27]).diff(moment([2007, 0, 29]));
// => -172800000
moment([2007, 0, 27]).diff(moment([2007, 0, 29]), 'days');
// => -2

// date-fns
import differenceInMilliseconds from 'date-fns/differenceInMilliseconds';
differenceInMilliseconds(new Date(2007, 0, 27), new Date(2007, 0, 29));
// => -172800000
import differenceInDays from 'date-fns/differenceInDays';
differenceInDays(new Date(2007, 0, 27), new Date(2007, 0, 29));
// => -2

// dayjs
dayjs('2007-01-27').diff(dayjs('2007-01-29'), 'milliseconds');
// => -172800000
dayjs('2007-01-27').diff(dayjs('2007-01-29'), 'days');
// => -2
```

<br>


## 查询

### 是否之前

检查日期是否在另一个日期之前。

```js
// Moment.js
moment('2010-10-20').isBefore('2010-10-21');
// => true

// date-fns
import isBefore from 'date-fns/isBefore';
isBefore(new Date(2010, 9, 20), new Date(2010, 9, 21));
// => true

// dayjs
dayjs('2010-10-20').isBefore('2010-10-21');
// => true
```

<br>


### 是否一样

检查日期是否与另一个日期相同。

```js
// Moment.js
moment('2010-10-20').isSame('2010-10-21');
// => false
moment('2010-10-20').isSame('2010-10-20');
// => true
moment('2010-10-20').isSame('2010-10-21', 'month');
// => true

// date-fns
import isSameDay from 'date-fns/isSameDay';
import isSameMonth from 'date-fns/isSameMonth';
isSameDay(new Date(2010, 9, 20), new Date(2010, 9, 21));
// => false
isSameDay(new Date(2010, 9, 20), new Date(2010, 9, 20));
// => true
isSameMonth(new Date(2010, 9, 20), new Date(2010, 9, 21));
// => true

// dayjs
dayjs('2010-10-20').isSame('2010-10-21');
// => false
dayjs('2010-10-20').isSame('2010-10-20');
// => true
// dayjs ❌  不支持
```
<br>

### 是否之后

检查日期是否在另一个日期之后。

```js
// Moment.js
moment('2010-10-20').isAfter('2010-10-19');
// => true

// date-fns
import isAfter from 'date-fns/isAfter';
isAfter(new Date(2010, 9, 20), new Date(2010, 9, 19));
// => true

// dayjs
dayjs('2010-10-20').isAfter('2010-10-19');
// => true
```
<br>


### 是否在两个日期之前

检查日期是否在两个其他日期之间。

```js
// Moment.js
moment('2010-10-20').isBetween('2010-10-19', '2010-10-25');
// => true

// date-fns
import isWithinInterval from 'date-fns/isWithinInterval';
isWithinInterval(new Date(2010, 9, 20), {
  start: new Date(2010, 9, 19),
  end: new Date(2010, 9, 25),
});
// => true

// dayjs ⚠️ requires isBetween plugin
import isBetween from 'dayjs/plugin/isBetween';
dayjs.extend(isBetween);
dayjs('2010-10-20').isBetween('2010-10-19', '2010-10-25');
// => true
```

<br>

### 是否是闰年

判断是否是润年。

```js
// Moment.js
moment([2000]).isLeapYear();
// => true

// date-fns
import isLeapYear from 'date-fns/isLeapYear';
isLeapYear(new Date(2000, 0, 1));
// => true

// dayjs ⚠️ 要引入 isLeapYear 插件
import isLeapYear from 'dayjs/plugin/isLeapYear';
dayjs.extend(isLeapYear);
dayjs('2000').isLeapYear();
// => true
```
<br>

### 是否是日期对象

检查变量是否是js Date对象。

```js
// Moment.js
moment.isDate(new Date());
// => true

// date-fns
import isDate from 'date-fns/isDate';
isDate(new Date());
// => true

// dayjs ❌  不支持
```
<br>

## 总结
如果你只需要简单的操作那么`day.js`更适合你，如果复杂一点的项目那么`date-fns`看起来更合适。

## 关于
本文由jon-millent译 
* github https://github.com/Jon-Millent
* blog http://chiyude.fan
* 原文地址 https://github.com/you-dont-need/You-Dont-Need-Momentjs
<img src="https://user-gold-cdn.xitu.io/2018/9/18/165eb22f112a51b3?w=2800&h=800&f=jpeg&s=149546">
