# JavaScript中闭包和this的一些理解


### 循环与闭包

在一次面试中，面试官出了这样一道题，让我说出运行结果

```javascript
for(var i=1;i<=5;i++){
    setTimeout(()=>{
        console.log(i);
    },i*1000);
}
```
<!--more-->

这段代码在运行时会以每秒一次的频率输出五次 6。

首先，延迟函数的回调会在循环结束时才执行(具体内容可以了解一下事件循环)。根据作用域的工作原理，它们都被封闭在一个共享的全局作用域中，因此实际上只有一个 `i`，所有函数共享一个 `i` 的引用。

可以按照下面来改进：

```javascript
for (var i=1; i<=5; i++) {
	(function(j) {
		setTimeout(()=>{
			console.log( j );
		}, j*1000 );
	})( i );
}
//打印1到5
```

`IIFE`(立即调用函数表达式)会为每个迭代都生成一个新的作用域，使得延迟函数的回调可以将新的 作用域封闭在每个迭代内部，每个迭代中都会含有一个具有正确值的变量供我们访问。

如果你知道ES6的`let`，特点是具有块作用域，`for`循环的`let`每次迭代都会用上一次迭代结束的值初始化这个变量，所以还可以这样写：

```javascript
for (let i=1; i<=5; i++) {
	setTimeout(()=>{
		console.log( i );
	}, i*1000 );
}
//打印1到5
```

### 普通匿名函数和箭头函数的`this`

`this` 对象是在运行时基于函数的执行环境绑定的：在全局函数中，`this` 等于 `window`，而当函数被作为某个对象的方法调用时，`this` 等于那个对象。绑定`this`指向等问题这里不做讨论。

在匿名函数中使用 `this` 对象也可能会导致一些问题。匿名函数的执行环境具有全局性，其` this` 对象通常指向` window`，而在严格模式下则是`undifined`。

```javascript
var name = "The Window";
var object = {
	name : "My Object",
	getNameFunc : function(){
		return function(){
			return this.name;
		};
 	}
};
alert(object.getNameFunc()()); //"The Window"（在非严格模式下）
```

箭头函数的函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。箭头函数可以让`setTimeout`里面的`this`，绑定定义时所在的作用域

```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```

如果是普通函数，执行时`this`应该指向全局对象`window`，这时应该输出`21`

```javascript
function foo() {
  setTimeout(function() {
    console.log('id:', this.id);
  }, 100);
}
var id = 21;
foo.call({ id: 42 });
// id: 21
```

箭头函数的`this`就是外层作用域的`this`，所以它的`this`指向是相对固定化的。
