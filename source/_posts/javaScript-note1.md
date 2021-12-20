---
title: JS相关笔记
date: 2021-11-09 12:00:08
tags: [JavaScript]
---

平时在学习过程中以及工作中积累的一些知识小点，俗话说好记不如烂笔头，东放西藏也不是道理，特在此集中记下，奥利给^_^
<!-- more -->

### 自定义触发事件 dispatchEvent()
- document.createEvent() 创建新的Event对象
- event.initEvent() 初始化
- element.dispatchEvent()

```javascript
var dom = document.querySelector('#id')
dom.addEventListener('alert', function (event) {
  console.log(event)
}, false);
 
// 创建
var evt = document.createEvent("HTMLEvents");
// 初始化
evt.initEvent("alert", false, false);
 
// 触发, 即弹出文字
dom.dispatchEvent(evt);
```

### js发起自定义事件 CustomEvent()
```javascript
new CustomEvent(eventName, params);
```

### JS获取背景图片

````javascript
    // 根据dom获取背景图片
      getBgImgs (dom) {
        const srcChecker = /url\(\s*?['"]?\s*?(\S+?)\s*?["']?\s*?\)/i
        return Array.from(
          Array.from(dom.querySelectorAll('*'))
            .reduce((collection, node) => {
              let prop = window.getComputedStyle(node, null)
                .getPropertyValue('background-image')
              let match = srcChecker.exec(prop)
              if (match) {
                collection.add(match[1])
              }
              return collection
            }, new Set())
        )
      }
````

##  Math.sqrt
返回一个数的平方根

## 高内聚低耦合
- 耦合： 软件模块之间相互依赖的程度。 侧重外交
  - 每次调用方法 A 之后都需要同步调用方法 B，那么此时方法 A 和 B 间的耦合度是高的。
- 内聚： 模块内的元素具有的共同点的相似程度。 侧重内功
  - 一个类中的多个方法有很多的共同之处，都是做支付相关的处理，那么这个类的内聚度是高的。
- 单一职责原则： 一个类 一个function只做一件事情
软件中每个元素都只完成自己职责的事情，将其他的事情交给别人

## 实现JS异步编程的4种方法
- 下面几种解决方式从根本上来说都是利用了浏览器定时器的工作原理

一、 回调函数
- 异步编程中最基本的方式；采用这种方式，把同步操作变成了异步操作，f1不会堵塞程序运行，相当于先执行程序的主要逻辑，将耗时的操作推迟执行。
- 利用定时器的工作原理将f1放入事件队列中去执行，哪怕延时是0，也是如此，因此不堵塞程序运行。此处掌握定时器的工作原理。
- 回调函数的优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度耦合，流程会很混乱，而且每个任务只能指定一个回调函数。

```typescript
fn1(fn2());
const fn1 = (callback: any) => {
  console.log('这是方法fn1的内部')
  setTimeout(() => {
        callback()
    }, 300);
};
const fn2 = () => {
    console.log('这是方法fn2的内部')
}

```

二、 事件监听
- 不取决于代码的顺序，在于这个事件是否被触发执行；

三、 发布/订阅
- "发布/订阅模式"（publish-subscribe pattern），又称"观察者模式"（observer pattern）
- 假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号，
其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。

四、 Promises对象
![工业聚：100 行代码实现 Promises/A+ 规范](https://mp.weixin.qq.com/s/qdJ0Xd8zTgtetFdlJL3P1g)
- 每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数
- f1().then(f2).then(f3);

五、 async/await
async/await，async 是 Generator 函数的语法糖，async返回的是promise对象，await控制执行顺序。
async/await 的优点是代码清晰（不像使用 Promise 的时候需要写很多 then 的方法链），可以处理回调地狱的问题。
async/await 写起来使得 JS 的异步代码看起来像同步代码，其实异步编程发展的目标就是让异步逻辑的代码看起来像同步一样容易理解。

六、 Generator 生成器
Generator 也是一种异步编程解决方案，它最大的特点就是可以交出函数的执行权，
Generator 函数可以看出是异步任务的容器，需要暂停的地方，都用 yield 语法来标注
Generator 函数一般配合 yield 使用，Generator 函数最后返回的是迭代器

## 回调函数是什么? 如何解决地狱回调?
- 回调函数: 本质上是一个闭包
  被作为实参传入另一函数，并在该外部函数内被调用，用以来完成某些任务的函数，称为回调函数
  一个回调函数，也被称为高阶函数，是一个被作为参数传递给另一个函数（在这里我们把另一个函数叫做“otherFunction”）的函数，
  回调函数在其他方法函数中被调用。一个回调函数本质上是一种编程模式（为一个常见问题创建的解决方案），因此，使用回调函数也叫做回调模式
- 回调地狱: 因为回调太多导致代码可读性变差
解决方案:
1. 给函数命名传递名字作为回调函数, 不在主函数中的参数中定义匿名函数
2. 模块化- 将代码相同功能的代码单独放入一个模块中，这样你就可以导出一块代码来完成特定的工作。然后在应用中导入模块

## generator如何控制函数执行?
yield

## 防抖
- 一段时间内只执行一次,不断的操作中（输入、点击等）最终只执行一次的一种提高性能的方法;
- 假定间隔时间设置10秒, 10s内疯狂点击,最终只会执行一次, 执行完之后清零等待下一次的点击操作

```typescript
/**
 * 
 * @param fn 方法
 * @param delay 时间(毫秒), 假定300毫秒为无意识的触发
 */
// 防抖函数（简洁版）
function debounce(fn,delay){
  let timer = null //借助闭包
  return function() {
    // 每次执行前先清除定时器，以确保在delay时间内fn函数不被执行。
    timer && clearTimeout(timer)
    timer = setTimeout(fn, delay)
  }
}
// 原始函数
const handlerScroll = function() {
  var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
  console.log('滚动条当前位置：' + scrollTop);
}
// 两函数结合，实现滚动防抖
const scrollHandler = debounce(handlerScroll, 1000)
window.addEventListener('scroll', scrollHandler)

```

## 节流
- 一段时间内只会执行一次
- 假定间隔时间设置10s, 每个10s就会执行一次方法, 即便10s内点击多次也只会执行一次
```typescript
/**
 * 一段时间内不断操作而在设定的规定时间内只执行一次
 * @param fn 点击的时间
 * @param delay 间隔时间(毫秒)
 */
function throttle(fn, delay) {
  let timer = null;      //定义一个定时器
  return function() {
    let context = this;
    let args = arguments;
    if(!timer) {
      timer = setTimeout(function() {
        fn.apply(context, args);
        timer = null;
      }, delay);
    }
  }
}
// 原始函数
const scrollEvent = function() {
  console.log('当前时间戳：' + new Date().getTime());
}
// 两函数结合，实现节流防抖
const scrollHandler = throttle(scrollEvent, 1000)
// 滚动事件
window.addEventListener('scroll', scrollHandler);

```

## Symbol
ES6中引入的新的原始数据类型，表示独一无二的值，最大的用法是用来定义对象的唯一的属性名。
- Symbol不能使用new命令，因为symbol是原始数据不是对象，可以接受一个字符串作为参数，提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分

```javascript
const domSymbol = Symbol('watermark-dom');
```

## 水印
watermark-dom
![简单水印与算法水印](https://segmentfault.com/a/1190000019285422)

```typescript
import { getCurrentInstance, onBeforeUnmount, ref, Ref, unref } from 'vue';

const domSymbol = Symbol('watermark-dom');

export function useWatermark(appendEl: Ref<HTMLElement | null> = ref(document.body)) {
  let func: Fn = () => {};
  const id = domSymbol.toString();
  const clear = () => {
    const domId = document.getElementById(id);
    if (domId) {
      const el = unref(appendEl);
      el && el.removeChild(domId);
    }
    window.removeEventListener('resize', func);
  };
  const createWatermark = (str: string) => {
    clear();

    // 创建画布
    const can = document.createElement('canvas');
    // 这是画布宽高
    can.width = 300;
    can.height = 240;

    const cans = can.getContext('2d');
    if (cans) {
      // 设置倾斜角度
      cans.rotate((-15 * Math.PI) / 120);
      // 设置字体
      cans.font = '15px 微软雅黑';
      // 设置填充颜色
      cans.fillStyle = 'rgba(0, 0, 0, 0.15)';
      // 设置文本内容的对齐方式
      cans.textAlign = 'left';
      // 设置在绘制文本是使用的当前文本基线
      cans.textBaseline = 'middle';
      // 在画布上绘制填色的文本 （开始X坐标，Y坐标）
      cans.fillText(str, can.width / 20, can.height);
    }

    const div = document.createElement('div');
    div.id = id;
    div.style.pointerEvents = 'none';
    div.style.top = '0px';
    div.style.left = '0px';
    div.style.position = 'absolute';
    div.style.opacity = '0.2';
    div.style.zIndex = '100000';
    div.style.width = document.documentElement.clientWidth + 'px';
    div.style.height = document.documentElement.clientHeight + 'px';
    div.style.background = 'url(' + can.toDataURL('image/png') + ') left top repeat';
    const el = unref(appendEl);
    el && el.appendChild(div);
    return id;
  };

  function setWatermark(str: string) {
    createWatermark(str);
    func = () => {
      createWatermark(str);
    };
    window.addEventListener('resize', func);
    const instance = getCurrentInstance();
    if (instance) {
      onBeforeUnmount(() => {
        clear();
      });
    }
  }

  return { setWatermark, clear };
}

```

## dom节点移动
sortablejs 插件
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

### 空值(null)判断运算符 ??（es11）
- 空值合并运算符（??）
  是一个逻辑操作符，当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数

### 可选链运算符 ?.（es11）
- 可选链运算符( ?. )
  允许读取位于连接对象链深处的属性的值，不必明确验证链中的每个引用是否有效
  
## 复合赋值运算符（es12）
op: 表达式
- 基本运算符
  - a op= b 等同于是 a = a op b
  
```typescript
// 例子
a += b // 等同于 a = a + b;
```

- 逻辑运算符和其他的复合赋值运算符工作方式不同
  - a op= b 等同于 a = a op (a = b)
 
 ```typescript
// 例子
a ||= b //等价于 a = a || (a = b)
a &&= b //等价于 a = a && (a = b)
a ??= b //等价于 a = a ?? (a = b)
```

- 复合赋值运算符的应用
  ??=可用来补充/初始化缺失的属性

 ```typescript
const pages = [
  { name: '1', age: 18, desc: '很开心第一次见面！' },
  { name: '2', age: 18 },
];

for (const page of pages){
    page.desc ??= '自我介绍';
}
```

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

### some()
数组中只要有一个满足就返回true,

### every()
必须所有都返回true才会返回true，哪怕有一个false，就会返回false

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