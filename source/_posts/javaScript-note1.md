---
title: JS相关笔记
date: 2021-11-09 12:00:08
tags: [JavaScript]
---

平时在学习过程中以及工作中积累的一些知识小点，俗话说好记不如烂笔头，东放西藏也不是道理，特在此集中记下，奥利给^_^
<!-- more -->

## dom节点移动
```javascript
  dragsData: {
    start: {}, // 移动的节点
    move: {}, // 移动后的位置所在
  }, // 移动的数据

    // 开始移动
    dragstart(value) {
      Object.assign(this.dragsData, {
        start: value,
      });
    },
    // 记录移动过程中信息
    dragenter(value, e) {
      Object.assign(this.dragsData, {
        move: value,
      });
      e.preventDefault();
      },
    // 拖拽最终操作
    dragend() {
      const { start, move } = this.dragsData;
      if (start.id !== move.id) {
        let oldIndex = this.newGroupArr.findIndex((f) => f.id === start.id);
        let newIndex = this.newGroupArr.findIndex((f) => f.id === move.id)
        let newItems = this.newGroupArr;
        // 删除开始移动的节点的的节点
        newItems.splice(oldIndex, 1);
        // 在列表中目标位置增加新的节点; // 即把开始选中移动的节点插入
        newItems.splice(newIndex, 0, start);
        this.newGroupArr = [...newItems]
      }
    },
    // 拖动事件（主要是为了拖动时鼠标光标不变为禁止）
    dragover(e) {
      e.preventDefault();
    },
```


## Reflect.has
- Reflect.has 用于检查一个对象是否拥有某个属性， 相当于in 操作符 。
- Reflect.has(target, propertyKey)
- 参数：
  - 目标对象: target
  - 属性名: propertyKey (需要检查目标对象是否存在此属性)
- 返回值：
  - 返回一个布尔值判断是否存在该属性
  - 如果属性存在于原型链中返回值为true
  
```typescript
Reflect.has(target, propertyKey)
```

## 插入表情

```typescript
insertEmoj (emoji: string) {
   const fieldRef = (this.$refs.replyField as any);
   const fieldDom = fieldRef.$el.getElementsByTagName('textarea')[0];
   const start = fieldDom.selectionStart;
   const end = fieldDom.selectionEnd;
   const text = fieldDom.value;
   if (start === undefined || end === undefined) return
   const result = text.substring(0, start) + emoji + text.substring(end);
   fieldDom.value = result;
   fieldRef.focus();
   fieldDom.selectionStart = start + emoji.length;
   fieldDom.selectionEnd = start + emoji.length;
   this.inputComments = result;
}
```

## 获取近一周的时间
```typescript
[...Array(7).keys()].map(days => new Date(Date.now() -( 24 * 60 * 60 * 1000) * days))
```

## 生成随机数
```typescript
Math.random().toString(36).substring(2);
```

## 添加参数到URL链接
```typescript
/**
 * Add the object as a parameter to the URL
 * @param baseUrl url
 * @param obj
 * @returns {string}
 * eg:
 *  let obj = {a: '3', b: '4'}
 *  setObjToUrlParams('www.baidu.com', obj)
 *  ==>www.baidu.com?a=3&b=4
 */
export function setObjToUrlParams(baseUrl: string, obj: any): string {
  let parameters = '';
  for (const key in obj) {
    parameters += key + '=' + encodeURIComponent(obj[key]) + '&';
  }
  parameters = parameters.replace(/&$/, '');
  return /\?$/.test(baseUrl) ? baseUrl + parameters : baseUrl.replace(/\/?$/, '?') + parameters;
}
```

## 获取URL的查询参数
```typescript
q={};location.search.replace(/([^?&=]+)=([^&]+)/g,(_,k,v)=>q[k]=v);q;
```

## 生成随机十六进制代码
```typescript
'#' + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, '0');
```

## 对象
### Object.key
- 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

```javascript
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo is a property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  }
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```

### Object.values
- 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值
```javascript
let person = {name:"老王",age:25,address:"重庆",getName:function(){}};
console.log(Object.keys(person)); // ["name","age", "address", "getName"]
```

### 空值合并运算符 ??
- 空值合并运算符（??）
  是一个逻辑操作符，当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数

### 可选链运算符 ?.
- 可选链运算符( ?. )
  允许读取位于连接对象链深处的属性的值，不必明确验证链中的每个引用是否有效
  
### 变换变量的值
```typescript
let a=1;let b=2;
[a,b]=[b,a];
```

## Number

### 数字分隔符
```typescript
const myMoney = 1_000_000_000_000

console.log(myMoney) // 1000000000000
```

### 金额格式化
```typescript
/**
 * 金额格式化
 * @param {any} num: 金额
 */
export function formatMoney(num: number | string): string {
  if (!num) {
    return <string>num;
  }
  const nums = num.toString().split('.');
  return nums[0].replace(/\B(?=(\d{3})+(?!\d))/g, ',') + (nums[1] ? '.' + nums[1] : '');
}
```

### 大数字单位转化成 千、万、千万、亿
```typescript
/**
 * 大数字单位转化成 千、万、千万、亿
 * @param {any} value: 数字
 */
export function formatNumber(value: number, suffix = '次') {
  // formatMoney(value);
  const newValue = ['', '', suffix];
  let fr = 1000; // 以千为基数
  let num = 3; // 0的个数
  while (value / fr >= 1) {
    fr *= 10;
    num += 1;
  }
  if (num <= 4) {
    // 千
    newValue[1] = '千';
    newValue[0] = parseFloat(value / 1000) + '';
  } else if (num <= 8) {
    // 万
    const text1 = parseFloat(num - 4) / 3 > 1 ? '千万' : '万';
    const fm = '万' === text1 ? 10000 : 10000000;
    newValue[1] = text1;
    newValue[0] = value / fm + '';
  } else if (num <= 16) {
    // 亿
    let text1 = (num - 8) / 3 > 1 ? '千亿' : '亿';
    text1 = (num - 8) / 4 > 1 ? '万亿' : text1;
    text1 = (num - 8) / 7 > 1 ? '千万亿' : text1;
    let fm = 1;
    if ('亿' === text1) {
      fm = 100000000; // 9位-8个0
    } else if ('千亿' === text1) {
      fm = 100000000000;
    } else if ('万亿' === text1) {
      fm = 1000000000000;
    } else if ('千万亿' === text1) {
      fm = 1000000000000000;
    }
    newValue[1] = text1;
    newValue[0] = parseFloat(value / fm) + '';
  }
  if (value < 1000) {
    newValue[1] = '';
    newValue[0] = value + '';
  }
  return newValue.join('');
}
```

### 去除小数 返回整数
```javascript
// 第一种
parseInt(10.99 )
console.log( parseInt(10.99 )) // 10

// 第二种
~~10.99
console.log(~~10.99) // 10
  
// 第三种
Math.trunc(10.99)
console.log(Math.trunc(10.99)) // 10
```

## 数组
### 对象数组去重

```typescript
/**
 * 对象数组去重
 * @param {any} array:数组
 * @param {any} field:去重字段
 */
export function arrayToDistinct(array, field) {
  const obj = {};
  array = array.reduce((cur, next) => {
    obj[next[field]] ? '' : (obj[next[field]] = true && cur.push(next));
    return cur;
  }, []); //设置cur默认类型为数组,并且初始值为空的数组
  return array;
}
/**
 * 对象数组去重,type表示对象里面的一个属性
 */
export function uniqueFun(arr, type) {
  const res = new Map();
  return arr.filter((a) => !res.has(a[type]) && res.set(a[type], 1));
}
```

### 数组最大值
```javascript

const arr = [1, 2, 3, 4]
// 第一种
console.log('第一种 maxNUm', Math.max(...arr ));
// 第二种
const maxNUm = Math.max.apply(this,arr );
console.log('第二种 maxNUm', maxNUm);
// 第三种
const maxNUm1 = arr .reduce((prev, cur) => {
    return Math.max(prev, cur);
    }, 0);
console.log('第三种 maxNUm', maxNUm1);
```
  
### 数组元素取法

```javascript
const arr = [0,1,2,3];
  // 取第一个元素
  let start = arr [0]
  // 等价于
  start = arr.shift();
  
  // 取最后一个元素
  let last = arr [arr.length -1];
  // 等价于
  last = arr.pop()
```

## 日期

### 结合moment.js
```typescript
// 预设时间范围
const weekOfDay = parseInt(moment().format('d')); // 计算今天是这周第几天 周日为一周中的第一天
const lastWeekStart = moment('00:00:00', 'HH:mm:ss').subtract(weekOfDay + 6, 'days'); // 上周周一日期
const lastWeekEnd = moment('00:00:00', 'HH:mm:ss').subtract(weekOfDay, 'days'); // 上周周日日期
const start = moment('00:00:00', 'HH:mm:ss').subtract(weekOfDay - 1, 'days'); // 本周周一日期
const lastMonthStart = moment('00:00:00', 'HH:mm:ss').subtract('month', 1) + '-01';
const lastMonthEnd = moment(lastMonthStart).subtract('month', -1).add('days', -1);

// 预设时间选项
const today = [moment('00:00:00', 'HH:mm:ss'), moment('23:59:59', 'HH:mm:ss')]; // 当天
const currentWeek = [start, moment('23:59:59', 'HH:mm:ss').endOf('week')]; // 本周
export const currentMonth = [moment().startOf('month'), moment('23:59:59', 'HH:mm:ss').endOf('month')]; // 本月
export const lastMonth = [
  moment('00:00:00', 'HH:mm:ss').subtract('month', 1).date(1), // 获取上月第一天
  moment('23:59:59', 'HH:mm:ss').subtract('month', 1).endOf('month'), // 获取上月最后一天；第一种写法
  // moment('23:59:59', 'HH:mm:ss').subtract('month').date(0), // 获取上月最后一天；第二种写法
]; // 上月

// 范围快捷设置
const setRanges = {
  今天: today,
  本周: currentWeek,
  上周: [lastWeekStart, lastWeekEnd],
  本月: currentMonth,
  上月: lastMonth,
};

/**
 * 根据某个时间 获取日期范围
 * @param currentDate 当前时间
 * @param dates 差距时间
 * @param day 时间范围 如15天
 */
const diffDate = (currentDate, dates, day) => {
  const diffDate = currentDate.diff(dates, 'days');
  return Math.abs(diffDate) > day;
};
```

### 不使用第三方库
```typescript
/**
 * 日期格式化
 * @param obj 时间
 * @param format 日期格式，不传默认返回年月日时分秒，例：2021-11-09 17:11:00
 */
export function formatTime(obj, format) {
  if (format) {
    let date;
    if (obj instanceof Date) {
      date = obj;
    } else {
      obj = obj.toString().length === 10 ? parseInt(obj) * 1000 : obj;
      date = new Date(obj);
    }
    const dayNames = ['星期一', '星期二', '星期三', '星期四', '星期五', '星期六', '星期日'];

    const o = {
      'M+': date.getMonth() < 9 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1, // 月份
      'd+': date.getDate() < 10 ? '0' + date.getDate() : date.getDate(), // 日
      'h+': date.getHours(), // 小时
      'm+': date.getMinutes(), // 分
      's+': date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds(), // 秒
      'q+': Math.floor((date.getMonth() + 3) / 3), // 季度
      'S+': date.getMilliseconds(), // 毫秒
      'D+': dayNames[date.getDay()], // 星期
    };

    if (/(y+)/.test(format)) format = format.replace(RegExp.$1, `${date.getFullYear()}`.substr(4 - RegExp.$1.length));
    for (const k in o) {
      if (new RegExp(`(${k})`).test(format)) {
        format = format.replace(RegExp.$1, RegExp.$1.length === 1 ? o[k] : `00${o[k]}`.substr(`${o[k]}`.length));
      }
    }
    return format;
  } else {
    obj = obj.toString().length === 10 ? parseInt(obj) * 1000 : obj;
    const date = new Date(obj);
    const _year = date.getFullYear();
    const _month = date.getMonth() + 1 > 9 ? date.getMonth() + 1 : '0' + (date.getMonth() + 1);
    const _date = date.getDate() > 9 ? date.getDate() : '0' + date.getDate();
    const _hour = date.getHours() > 9 ? date.getHours() : '0' + date.getHours();
    const _minute = date.getMinutes() > 9 ? date.getMinutes() : '0' + date.getMinutes();
    const _second = date.getSeconds() > 9 ? date.getSeconds() : '0' + date.getSeconds();
    return _year + '-' + _month + '-' + _date + ' ' + _hour + ':' + _minute + ':' + _second;
  }
}
```

## 文件

### 图片

#### base64 to blob
```typescript
/**
 * @description: base64 to blob
 */
export function dataURLtoBlob(base64Buf: string): Blob {
  const arr = base64Buf.split(',');
  const typeItem = arr[0];
  const mime = typeItem.match(/:(.*?);/)![1];
  const bstr = atob(arr[1]);
  let n = bstr.length;
  const u8arr = new Uint8Array(n);
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n);
  }
  return new Blob([u8arr], { type: mime });
}
```

#### img url to base64
```typescript
/**
 * img url to base64
 * @param url
 */
export function urlToBase64(url: string, mineType?: string): Promise<string> {
  return new Promise((resolve, reject) => {
    let canvas = document.createElement('CANVAS') as Nullable<HTMLCanvasElement>;
    const ctx = canvas!.getContext('2d');

    const img = new Image();
    img.crossOrigin = '';
    img.onload = function () {
      if (!canvas || !ctx) {
        return reject();
      }
      canvas.height = img.height;
      canvas.width = img.width;
      ctx.drawImage(img, 0, 0);
      const dataURL = canvas.toDataURL(mineType || 'image/png');
      canvas = null;
      resolve(dataURL);
    };
    img.src = url;
  });
}
```

#### img url to blob
```typescript
/**
 * img url to blob
 * @param url
 */
export function urlToBlob(url: string): Promise<string> {
  return new Promise((resolve) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url, true);
    xhr.responseType = 'blob';
    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(xhr.response);
      }
    };
    xhr.send();
  });
}
```