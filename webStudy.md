# 前端

[MDN官方文档](https://developer.mozilla.org/zh-CN/docs/Web)

## 1.html

## 2.CSS

## 3.js

### 3.1 js的调用与执行顺序

### 3.8函数

**定义方式**

```javascript
function add(a, b) {
    return a + b;
}

let add = function (a, b) {
    return a + b;
}

let add = (a, b) => {
    return a + b;
}
```

**返回值**

如果未定义返回值，则返回`undefined`。



### 3.9类

与`C++`中的`Class`类似。但是不存在私有成员。

- `this`指向类的实例。

**定义**

```javascript
class Point {
    constructor(x, y) {  // 构造函数
        this.x = x;  // 定义了一个成员变量
        this.y = y;
        this.init();
	}

	init() {
    	this.sum = this.x + this.y;  // 成员变量可以在任意的成员函数中定义
	}

	toString() {  // 成员函数
    	return '(' + this.x + ', ' + this.y + ')';
	}
}

let p = new Point(3, 4);
console.log(p.toString());
```

**继承**

```javascript
class ColorPoint extends Point {
    constructor(x, y, color) {
        super(x, y); // 这里的super表示父类的构造函数
        this.color = color;
    }
    
    toString() {
   		return this.color + ' ' + super.toString(); // 调用父类的toString()
	}
}
```

**注意：**

- ```javascript
  super
  ```

  这个关键字，既可以当作函数使用，也可以当作对象使用。

  - 作为函数调用时，代表父类的构造函数，且只能用在子类的构造函数之中。
  - `super`作为对象时，指向父类的原型对象。

- 在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字。

- 成员重名时，子类的成员会覆盖父类的成员。类似于`C++`中的多态。

**静态方法**

在成员函数前添加`static`关键字即可。静态方法不会被类的实例继承，只能通过类来调用。例如：

```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    
    toString() {
    	return '(' + this.x + ', ' + this.y + ')';
	}

	static print_class_name() {
    	console.log("Point");
	}
}

let p = new Point(1, 2);
Point.print_class_name();
p.print_class_name();  // 会报错
```

**静态变量**

在ES6中，只能通过`class.propname`定义和访问。例如：

```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        
        Point.cnt++;
	}

	toString() {
    	return '(' + this.x + ', ' + this.y + ')';
	}
}

Point.cnt = 0;

let p = new Point(1, 2);
let q = new Point(3, 4);

console.log(Point.cnt);
```

### 3.10 事件

JavaScript的代码一般通过事件触发。

可以通过`addEventListener`函数为元素绑定事件的触发函数。

常见的触发函数有：

**鼠标**

- `click`：鼠标左键点击

- `dblclick`：鼠标左键双击

- `contextmenu`：鼠标右键点击

- ```javascript
  mousedown
  ```

  ：鼠标按下，包括左键、滚轮、右键

  - `event.button`：0表示左键，1表示滚轮，2表示右键

- ```javascript
  mouseup
  ```

  ：鼠标弹起，包括左键、滚轮、右键

  - `event.button`：0表示左键，1表示滚轮，2表示右键

**键盘**

- ```
  keydown
  ```

  ：某个键是否被按住，事件会连续触发

  - `event.code`：返回按的是哪个键
  - `event.altKey`、`event.ctrlKey`、`event.shiftKey`分别表示是否同时按下了alt、ctrl、shift键。

- ```
  keyup
  ```

  ：某个按键是否被释放

  - `event`常用属性同上

- ```
  keypress
  ```

  ：紧跟在

  ```
  keydown
  ```

  事件后触发，只有按下字符键时触发。适用于判定用户输入的字符。

  - `event`常用属性同上

```
keydown`、`keyup`、`keypress`的关系类似于鼠标的`mousedown`、`mouseup`、`click
```

**表单**

- `focus`：聚焦某个元素
- `blur`：取消聚焦某个元素
- `change`：某个元素的内容发生了改变

**窗口**

需要作用到`window`元素上。

- `resize`：当窗口大小放生变化
- `scroll`：滚动指定的元素
- `load`：当元素被加载完成

### 3.11 常用库

#### 1.jQuery

**选择器**

```javascript
$(selector)，例如：
$('div');
$('.big-div');
$('div > p')
selector类似于CSS选择器。
```

**事件**

```javascript
//$(selector).on(event, func)绑定事件，例如：
$('div').on('click', function (e) {
    console.log("click div");
})
 
//$(selector).off(event, func)删除事件，例如：
$('div').on('click', function (e) {
    console.log("click div");
 
    $('div').off('click');// 事件被触发一次后就被解绑，之后不再触发
});
 
//当存在多个相同类型的事件触发函数时，可以通过click.name来区分，例如：
$('div').on('click.first', function (e) {
    console.log("click div");
 
    $('div').off('click.first');
}); 
```

**在事件触发的函数中的return false等价于同时执行：**

- e.stopPropagation()：阻止事件向上传递
  **比如说div中放了一个link，link和div都绑定了事件，那么点击link时候，div绑定的事件也会默认触发** ,阻止向上传递后就不会再触发div的事件

- e.preventDefault()：阻止事件的默认行为 
  **可 以阻止一个操作，但是不阻止事件向上传递**



 

**元素的隐藏、展现**

- $A.hide()：隐藏，可以添加参数，表示消失时间
- $A.show()：展现，可以添加参数，表示出现时间
- $A.fadeOut()：慢慢消失，可以添加参数，表示消失时间
- $A.fadeIn()：慢慢出现，可以添加参数，表示出现时间

**元素的添加、删除**

```html
$('<div class="mydiv"><span>Hello World</span></div>')           构造一个jQuery对象
```

$A.append($B)：将$B添加到$A的末尾
$A.prepend($B)：将$B添加到$A的开头
$A.remove()：删除元素$A
$A.empty()：清空元素$A的所有儿子

**对类的操作**

- $A.addClass(class_name)：添加某个类
- $A.removeClass(class_name)：删除某个类
- $A.hasClass(class_name)：判断某个类是否存在

**对CSS的操作**

- $("div").css("background-color")：获取某个CSS的属性
- $("div").css("background-color","yellow")：设置某个CSS的属性
- 同时设置多个CSS的属性：

```javascript
$('div').css({
    width: "200px",
    height: "200px",
    "background-color": "orange",
});
```

**对标签属性的操作**

- $('div').attr('id')：获取属性
- $('div').attr('id', 'ID')：设置属性

**对HTML内容、文本的操作。（不需要背每个标签该用哪种，用到的时候Google或者百度即可）**

- $A.html()：获取、修改HTML内容
- $A.text()：获取、修改文本信息
- $A.val()：获取、修改文本的值

**查找**

$(selector).parent(filter)：查找父元素,看看filter是不是查找元素的父元素，后面同理 
$(selector).parents(filter)：查找所有祖先元素
$(selector).children(filter)：查找儿子元素
$(selector).find(filter)：在所有后代元素中查找









**ajax**

**GET方法：**在不刷新页面的前提下，只从服务器端获取某些数据，一般是获取一个jaso

```javascript
$.ajax({
    url: url,
    type: "GET",
    data: {
    },
    dataType: "json",
    success: function (resp) {
 
    },
});
```

**POST方法：**

```javascript
$.ajax({
    url: url,
    type: "POST",
    data: {
    },
    dataType: "json",
    success: function (resp) {
 
    },
});
```





**setTimeout与setInterval**

setTimeout(func, delay)——delay毫秒后，执行函数func()。
setInterval(func, delay)——每隔delay毫秒，执行一次函数func()。第一次在第delay毫秒后执行。



#### 2.map与set

**Map：保存键值对。**

用for...of(遍历每个键值对)

```javascript
for(let [key,value] of map){
    console.log(key,value);
}
```

或者forEach可以按插入顺序遍历。
键值可以为任意值，包括函数、对象或任意基本类型。
**常用API：**

set(key, value)：插入键值对，如果key已存在，则会覆盖原有的value
get(key)：查找关键字，如果不存在，返回undefined
size：返回键值对数量
has(key)：返回是否包含关键字key
delete(key)：删除关键字key
clear()：删除所有元素

**Set：允许你存储任何类型的唯一值，无论是原始值或者是对象引用**。

用for...of或者forEach可以按插入顺序遍历。
常用API：

add()：添加元素
has()：返回是否包含某个元素
size：返回元素数量
delete()：删除某个元素
clear()：删除所有元素

#### 3.localStorage

> **可以在用户的浏览器上存储键值对。**
>
> 常用API：
>
> - setItem(key, value)：插入
> - getItem(key)：查找
> - removeItem(key)：删除
> - clear()：清空

#### JSON

> **JSON对象用于序列化对象、数组、数值、字符串、布尔值和null。**
>
> 常用API：
>
> - JSON.parse()：将字符串解析成对象
> - JSON.stringify()：将对象转化为字符串

#### 日期

返回值为整数的API，数值为1970-1-1 00:00:00 UTC（世界标准时间）到某个时刻所经过的毫秒数：

Date.now()：返回现在时刻。
Date.parse("2022-04-15T15:30:00.000+08:00")：返回北京时间2022年4月15日 15:30:00的时刻到时间零点的毫秒数。

**与Date对象的实例相关的API：**

new Date()：返回现在时刻。
new Date("2022-04-15T15:30:00.000+08:00")：返回北京时间2022年4月15日 15:30:00的时刻。
两个Date对象实例的差值为毫秒数
getDay()：返回星期，0表示星期日，1-6表示星期一至星期六
getDate()：返回日，数值为1-31
getMonth()：返回月，数值为0-11
getFullYear()：返回年份
getHours()：返回小时
getMinutes()：返回分钟
getSeconds()：返回秒
getMilliseconds()：返回毫秒



#### WebSocket
**与服务器建立全双工连接**。

常用API：

new WebSocket('ws://localhost:8080');：建立ws连接。
send()：向服务器端发送一个字符串。一般用JSON将传入的对象序列化为字符串。
onopen：类似于onclick，当连接建立时触发。
onmessage：当从服务器端接收到消息时触发。
close()：关闭连接。
onclose：当连接关闭后触发。

#### window

> - indow.open("https://www.acwing.com")在新标签栏中打开页面。
> - location.reload()刷新页面。
> - location.href = "https://www.acwing.com"：在当前标签栏中打开页面。

#### canvas

[Canvas教程](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)