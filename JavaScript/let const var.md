## 使用let与const来代替var的好处

### 1.不存在变量提升

JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后一行行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。

```javascript
console.log(a)
var a = 1
//undefined
```

上面一段代码中，在变量a声明之前打印a，按照正常代码运行逻辑应该会出错，但是在JavaScript中这样做并不会报错，原因是以上代码被解析成了以下代码

```javascript
var a
console.log(a)
a = 1
```

声明了变量a但是没有赋值，所有打印出来为undefined。

### 2.暂时性死区

在代码块内，使用let或const变量声明变量之前，该变量都是不可用的。这在语法上，叫***暂时性死区***。

```javascript
if(true){
  a = 'ooo' //ReferenceError
  let a 
}
```

ES6 规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。这样的错误在 ES5 是很常见的，现在有了这种规定，避免此类错误就很容易了。

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

### 3.不允许重复声明

let不允许在相同作用域内，重复声明同一个变量。

```javascript
// 报错
function func() {
  let a = 10;
  var a = 1;
}
// 报错
function func() {
  let a = 10;
  let a = 1;
}
```

因此，不能在函数内部重新声明参数。

```javascript
function func(arg) {
  let arg;
}
func() // 报错
function func(arg) {
  {
    let arg;
  }
}
func() // 不报错
```

### 4.块级作用域

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

 第一种场景，内层变量可能会覆盖外层变量。

```javascript
var tmp = new Date();
function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}
f(); // undefined
```

上面代码的原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

第二种场景，for循环用来计数的循环变量泄露为全局变量。

```javascript
var s = 'hello';
for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}
console.log(i); // 5
```

上面代码中，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

使用let、const为JavaScript带来了块级作用域

```javascript
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

上面的函数有两个代码块，都声明了变量n，运行后输出 5。这表示外层代码块不受内层代码块的影响。如果两次都使用var定义变量n，最后输出的值才是 10。

ES6 允许块级作用域的任意嵌套。

```javascript
{{{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}}}};
```

上面代码使用了一个五层的块级作用域，每一层都是一个单独的作用域。第四层作用域无法读取第五层作用域的内部变量。

内层作用域可以定义外层作用域的同名变量。

```javascript
{{{{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
}}}};
```

## const命令

const声明一个只读的常量。一旦声明，常量的值就不能改变。与let一样具有块级作用域，不存在变量提升，不允许重复声明，暂时性锁区的特点。



**参考文档**

[网道项目](https://wangdoc.com/)