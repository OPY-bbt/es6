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

数组的扩展
-----------
	#Array.from 
		将两类对象转为真正的数组。类似数组的对象（array-likeobject）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。

		实际应用中，常见的类似数组的对象是DOM操作返回的NodeList集合，以及函数内部的arguments对象。Array.from都可以将它们转为真正的数组。
		// NodeList对象
		let ps = document.querySelectorAll('p');
		Array.from(ps).forEach(function (p) {
		  console.log(p);
		});

		// arguments对象
		function foo() {
		  var args = Array.from(arguments);
		  // ...
		}

		Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

		如果map函数里面用到了this关键字，还可以传入Array.from的第三个参数，用来绑定this。


	#Array.of()
		用于将一组值，转换为数组。

	#[].copyWidthin()
		在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

		target（必需）：从该位置开始替换数据。
		start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
		end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

	#数组实例的find()和findIndex()
		[1, 4, -5, 10].find((n) => n < 0)
		// -5

		[1, 5, 10, 15].findIndex(function(value, index, arr) {
		  return value > 9;
		}) // 2

	#数组实例的fill()
		['a', 'b', 'c'].fill(7)
		// [7, 7, 7]

		['a', 'b', 'c'].fill(7, 1, 2)
		// ['a', 7, 'c']

	#数组实例的entries()，keys()和values()
		for (let index of ['a', 'b'].keys()) {
		  console.log(index);
		}
		// 0
		// 1

		for (let elem of ['a', 'b'].values()) {
		  console.log(elem);
		}
		// 'a'
		// 'b'

		for (let [index, elem] of ['a', 'b'].entries()) {
		  console.log(index, elem);
		}
		// 0 "a"
		// 1 "b"

	#数组实例的includes()
		[1, 2, 3].includes(2);     // true


数组的扩展
-----------
	#函数参数的默认值
		function log(x, y = 'world'){
			console.log(x,y);
		}

	#与解构赋值默认值结合使用
		function fetch(url, { body = '', method = 'GET', headers = {} }) {
		  console.log(method);
		}

		fetch('http://example.com', {})
		// "GET"

		fetch('http://example.com')

	#参数默认值的位置在尾参数

	#应用 利用个参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。
		function throwIfMissing() {
		  throw new Error('Missing parameter');
		}

		function foo(mustBeProvided = throwIfMissing()) {
		  return mustBeProvided;
		}

		foo()
		// Error: Missing parameter

	#rest 参数。
		function add(...values) {
			let sum = 0;

			for(let num of values) {
				sum += val;
			}

			return sum;
		}

		add(2,3,5) //10

		// arguments变量的写法
		function sortNumbers() {
		  return Array.prototype.slice.call(arguments).sort();
		}

		// rest参数的写法
		const sortNumbers = (...numbers) => numbers.sort();

	#扩展运算符
		##它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
		
		##代替数组的apply方法
			// ES5的写法
			function f(x, y, z) {
			  // ...
			}
			var args = [0, 1, 2];
			f.apply(null, args);

			// ES6的写法
			function f(x, y, z) {
			  // ...
			}
			var args = [0, 1, 2];
			f(...args);


			// ES5的写法
			Math.max.apply(null, [14, 3, 77])

			// ES6的写法
			Math.max(...[14, 3, 77])

			// 等同于
			Math.max(14, 3, 77);

	# 总结： 扩展运算符与rest参数 正好相反 ...后加变量转换为数组 后加数组转换为逗号相隔的参数。

	# 扩展运算符的应用
		## 合并数组
			es5 [1,2].concat(more);
			es6 [1,2,...more];

			var arr1 = [1];
			var arr2 = [2];
			var arr3 = [3];

			arr1.concat(arr2,arr3);
			[...arr1, ...arr2, ...arr3];

		## 与解构赋值结合
			const [first, ...rest] = [1,2,3,4];
			rest //[2,3,4];

		##函数的返回值
			var dateFields = readDateFields(database);
			var d = new Date(...dateFields);

		##字符串
			[...'hello']
			// [ "h", "e", "l", "l", "o" ]

	#name
		函数的name属性，返回该函数的函数名。

	# =>
		## 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
		## 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
		##由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。
			var getTempItem = id => ({ id: id, name: "Temp" });
		## 注意点
			1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。(内部没有this)
			2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
			3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
			4）不可以使用yield命令，因此箭头函数不能用作Generator函数。


对象的扩展
-----------
	#属性的简洁表示法
		var foo = 'bar';
		var baz = {foo};
		baz // {foo: "bar"}

		// 等同于
		var baz = {foo: foo};


		var o = {
		  method() {
		    return "Hello!";
		  }
		};

		// 等同于
		var o = {
		  method: function() {
		    return "Hello!";
		  }
		};

	#属性名表达式
		let propKey = 'foo';

		let obj = {
		  [propKey]: true,
		  ['a' + 'bc']: 123
		};

	#Object.assign()
		var target = { a: 1 };

		var source1 = { b: 2 };
		var source2 = { c: 3 };

		Object.assign(target, source1, source2);
		target // {a:1, b:2, c:3}

		Object.assign可以用来处理数组，但是会把数组视为对象。
		Object.assign([1, 2, 3], [4, 5])
		// [4, 5, 3]

		##用途
			1）为对象添加属性
				class Point {
				  constructor(x, y) {
				    Object.assign(this, {x, y});
				  }
				}

			2）克隆对象
				function clone(origin) {
				  return Object.assign({}, origin);
				}

				克隆继承的值
				function clone(origin) {
				  let originProto = Object.getPrototypeOf(origin);
				  return Object.assign(Object.create(originProto), origin);
				}

			3）合并多个对象
				const merge = (target, ...sources) => Object.assign(target, ...sources);

			4）为属性指定默认值
				const DEFAULTS = {
				  logLevel: 0,
				  outputFormat: 'html'
				};

				function processContent(options) {
				  options = Object.assign({}, DEFAULTS, options);
				  console.log(options);
				  // ...
				}

	#Object.setPrototypeOf（object, prototype）
		let proto = {};
		let obj = { x: 10 };
		Object.setPrototypeOf(obj, proto);

		proto.y = 20;
		proto.z = 40;

		obj.x // 10
		obj.y // 20
		obj.z // 40

	#Object.getPrototypeOf(obj);
		function Rectangle() {
		  // ...
		}

		var rec = new Rectangle();

		Object.getPrototypeOf(rec) === Rectangle.prototype
		// true

		Object.setPrototypeOf(rec, Object.prototype);
		Object.getPrototypeOf(rec) === Rectangle.prototype
		// false

	#Object.keys（）
		ES5 引入了Object.keys方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。

		ES2017 引入了跟Object.keys配套的Object.values和Object.entries，供for...of循环使用。

		let {keys, values, entries} = Object;
		let obj = { a: 1, b: 2, c: 3 };

		for (let key of keys(obj)) {
		  console.log(key); // 'a', 'b', 'c'
		}

		for (let value of values(obj)) {
		  console.log(value); // 1, 2, 3
		}

		for (let [key, value] of entries(obj)) {
		  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
		}