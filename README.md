let
---------
	#只在let命令所在的代码块内有效
		{
		  let a = 10;
		  var b = 1;
		}

		a // ReferenceError: a is not defined.
		b // 1

	#不存在变量提升
		// var 的情况
		console.log(foo); // 输出undefined
		var foo = 2;

		// let 的情况
		console.log(bar); // 报错ReferenceError
		let bar = 2;

	#暂时性死区
		var tmp = 123;

		if (true) {
		  tmp = 'abc'; // ReferenceError
		  let tmp;
		}

	#不允许重复声明

	#ES6 的块级作用域
		// IIFE 写法
		(function () {
		  var tmp = ...;
		  ...
		}());

		// 块级作用域写法
		{
		  let tmp = ...;
		  ...
		}


const
---------
	#只读的常量
		const PI = 3.1415;
		PI // 3.1415

		PI = 3;
		// TypeError: Assignment to constant variable
		const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

		const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。

	#如果真的想将对象冻结，应该使用Object.freeze方法。
		const foo = Object.freeze({});

		// 常规模式时，下面一行不起作用；
		// 严格模式时，该行会报错
		foo.prop = 123;

	#除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。
		var constantize = (obj) => {
		  Object.freeze(obj);
		  Object.keys(obj).forEach( (key, i) => {
		    if ( typeof obj[key] === 'object' ) {
		      constantize( obj[key] );
		    }
		  });
		};

数组解构赋值
---------
	let [foo, [[bar], baz]] = [1, [[2], 3]];
	foo // 1
	bar // 2
	baz // 3

	let [ , , third] = ["foo", "bar", "baz"];
	third // "baz"

	let [x, , y] = [1, 2, 3];
	x // 1
	y // 3

	let [head, ...tail] = [1, 2, 3, 4];
	head // 1
	tail // [2, 3, 4]

	let [x, y, ...z] = ['a'];
	x // "a"
	y // undefined
	z // []

对象解构赋值
---------
	let { foo, bar } = { foo: "aaa", bar: "bbb" };
	foo // "aaa"
	bar // "bbb"

字符串解构赋值
---------
	const [a, b, c, d, e] = 'hello';
	a // "h"
	b // "e"
	c // "l"
	d // "l"
	e // "o"

数值和布尔值的解构赋值
---------
	#解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
		let {toString: s} = 123;
		s === Number.prototype.toString // true

		let {toString: s} = true;
		s === Boolean.prototype.toString // true

函数参数的解构赋值
---------
		#函数参数的解构也可以使用默认值。
		function move({x = 0, y = 0} = {}) {
		  return [x, y];
		}

		move({x: 3, y: 8}); // [3, 8]
		move({x: 3}); // [3, 0]
		move({}); // [0, 0]
		move(); // [0, 0]

用途
---------
	#交换变量的值
		let x = 1;
		let y = 2;

		[x, y] = [y, x];

	#从函数返回多个值
		返回一个数组

		function example() {
		  return [1, 2, 3];
		}
		let [a, b, c] = example();

		返回一个对象

		function example() {
		  return {
		    foo: 1,
		    bar: 2
		  };
		}
		let { foo, bar } = example();

	#函数参数的定义
		// 参数是一组有次序的值
		function f([x, y, z]) { ... }
		f([1, 2, 3]);

		// 参数是一组无次序的值
		function f({x, y, z}) { ... }
		f({z: 3, y: 2, x: 1});

	#提取JSON数据
		let jsonData = {
		  id: 42,
		  status: "OK",
		  data: [867, 5309]
		};

		let { id, status, data: number } = jsonData;

		console.log(id, status, number);
		// 42, "OK", [867, 5309]

	#函数参数的默认值
		jQuery.ajax = function (url, {
		  async = true,
		  beforeSend = function () {},
		  cache = true,
		  complete = function () {},
		  crossDomain = false,
		  global = true,
		  // ... more config
		}) {
		  // ... do stuff
		};

	#遍历Map结构
		var map = new Map();
		map.set('first', 'hello');
		map.set('second', 'world');

		for (let [key, value] of map) {
		  console.log(key + " is " + value);
		}
		// first is hello
		// second is world

		如果只想获取键名，或者只想获取键值
			// 获取键名
			for (let [key] of map) {
			  // ...
			}

			// 获取键值
			for (let [,value] of map) {
			  // ...
			}

		输入模块的指定方法
			const { SourceMapConsumer, SourceNode } = require("source-map");

字符串的扩展
---------
	#repeat()
		'x'.repeat(3) // "xxx"
		'hello'.repeat(2) // "hellohello"
		'na'.repeat(0) // ""

	#padStart()，padEnd()
		'x'.padStart(5, 'ab') // 'ababx'
		'x'.padStart(4, 'ab') // 'abax'

		'x'.padEnd(5, 'ab') // 'xabab'
		'x'.padEnd(4, 'ab') // 'xaba'

		'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
		'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"

	
	#如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。

		如果你不想要这个换行，可以使用trim方法消除它

		模板字符串中嵌入变量，需要将变量名写在${}之中。

		模板字符串之中还能调用函数。
			function fn() {
			  return "Hello World";
			}

			`foo ${fn()} bar`
			// foo Hello World bar

正则的扩展
---------
	#es5中两种用法
		## var regex = new RegExp('xyz', 'i'); regex = /xyz/i;
		## var regex = new RegExp(/xyz/i); regex = /xyz/i;

	#es6中
	 	## var regex = new RegExp(/abc/ig, 'i')

	#字符串的正则
		全部绑定到RegExp的实例方法。

	#u修饰符
		可以正确处理四个字节的UTF-16

	#y修饰符
		和g类似, 不过y修饰符要确保匹配必须从剩余的第一个位置开始。
		var s = 'aaa_aa_a';
		var r1 = /a+/g;
		var r2 = /a+/y;

		r1.exec(s) // ["aaa"]
		r1.exec(s) // ["aa"]

		r2.exec(s) // ["aaa"]
		r2.exec(s) // null

	#flags
		/abc/ig.source
		// "abc"

		/abc/ig.flags
		// 'gi'

Number的扩展
---------
	#二进制和八进制表示法
		二进制 0b（0B）
		八进制 0o（0O）
		Number可以将0b和0o前缀的字符串数值转换为十进制。

	#Number.isFinite() Number.isNaN();

	#Number.parseInt() Number.parseFloat()

	#Number.isInteger() 注意25 和 25.0 都会返回true

	#Number.EPSILON 是一个极小的常量 在于为浮点数的计算，设置一个误差范围。

Math对象的扩展
-----------
	#Math.trunc() 返回整数部分

	#Math.sign()
		参数为正数，返回+1；
		参数为负数，返回-1；
		参数为0，返回0；
		参数为-0，返回-0;
		其他值，返回NaN。

	#Math.cbrt()
		立方根

	#Math.hypot()
		返回所有参数的平方和的平方根

	#Math.log10()
		返回以10为底的x的对数

	#Math.log2()
		返回以2为底的x的对数

	#指数运算符 **
		2 ** 3 = 8














