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


Symbol
-----------
	#  ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript语言的第七种数据类型，前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

	#  作为属性名的Symbol

	#有时，我们希望重新使用同一个Symbol值，Symbol.for方法可以做到这一点。


Set & Map
-----------	
	#set
		## ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
			Set 函数可以接受一个数组（或类似数组的对象）作为参数，用来初始化。
			数组去重	[...new Set(array)];
			两个对象总是不相等

		##属性
			Set.prototype.constructor：构造函数，默认就是Set函数。
			Set.prototype.size：返回Set实例的成员总数。

			add(value)：添加某个值，返回Set结构本身。
			delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
			has(value)：返回一个布尔值，表示该值是否为Set的成员。
			clear()：清除所有成员，没有返回值。

			keys()：返回键名的遍历器
			values()：返回键值的遍历器
			entries()：返回键值对的遍历器
			forEach()：使用回调函数遍历每个成员

			let a = new Set([1, 2, 3]);
			let b = new Set([4, 3, 2]);

			// 并集
			let union = new Set([...a, ...b]);
			// Set {1, 2, 3, 4}

			// 交集
			let intersect = new Set([...a].filter(x => b.has(x)));
			// set {2, 3}

			// 差集
			let difference = new Set([...a].filter(x => !b.has(x)));
			// Set {1}

	#Map
		## Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应

		##属性
			size

		##方法
			set(key, value) 可以链式调用
			get(key)
			has(key)
			delete(key)
			clear()

			遍历方法
				keys()：返回键名的遍历器。
				values()：返回键值的遍历器。
				entries()：返回所有成员的遍历器。
				forEach()：遍历Map的所有成员。

				let map = new Map([
				  [1, 'one'],
				  [2, 'two'],
				  [3, 'three'],
				]);

				[...map.keys()]
				// [1, 2, 3]

				[...map.values()]
				// ['one', 'two', 'three']

				[...map.entries()]
				// [[1,'one'], [2, 'two'], [3, 'three']]

				[...map]
				// [[1,'one'], [2, 'two'], [3, 'three']]

		##与其他数据结构的相互转换
			###Map转为数组
				let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
				[...myMap]
				// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]

			###数组转为Map
				new Map([[true, 7], [{foo: 3}, ['abc']]])
				// Map {true => 7, Object {foo: 3} => ['abc']}

			###Map转为对象
				如果所有Map的键都是字符串，它可以转为对象。
				function strMapToObj(strMap) {
				  let obj = Object.create(null);
				  for (let [k,v] of strMap) {
				    obj[k] = v;
				  }
				  return obj;
				}

				let myMap = new Map().set('yes', true).set('no', false);
				strMapToObj(myMap)
				// { yes: true, no: false }

			###对象转为Map
				function objToStrMap(obj) {
				  let strMap = new Map();
				  for (let k of Object.keys(obj)) {
				    strMap.set(k, obj[k]);
				  }
				  return strMap;
				}

				objToStrMap({yes: true, no: false})
				// [ [ 'yes', true ], [ 'no', false ] ]

Proxy
-----------
	var handler = {
	  get: function(target, name) {
	    if (name === 'prototype') {
	      return Object.prototype;
	    }
	    return 'Hello, ' + name;
	  },

	  apply: function(target, thisBinding, args) {
	    return args[0];
	  },

	  construct: function(target, args) {
	    return {value: args[1]};
	  }
	};

	var fproxy = new Proxy(function(x, y) {
	  return x + y;
	}, handler);

	fproxy(1, 2) // 1
	new fproxy(1,2) // {value: 2}
	fproxy.prototype === Object.prototype // true
	fproxy.foo // "Hello, foo"


Reflect
-----------
	# 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。

	# 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
	
	# 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
	
	# Reflect.get(target, name, receiver)
		var myObject = {
		  foo: 1,
		  bar: 2,
		  get baz() {
		    return this.foo + this.bar;
		  },
		}

		Reflect.get(myObject, 'foo') // 1
		Reflect.get(myObject, 'bar') // 2
		Reflect.get(myObject, 'baz') // 3

	# Reflect.set(target, name, value, receiver)
		var myObject = {
		  foo: 1,
		  set bar(value) {
		    return this.foo = value;
		  },
		}

		myObject.foo // 1

		Reflect.set(myObject, 'foo', 2);
		myObject.foo // 2

		Reflect.set(myObject, 'bar', 3)
		myObject.foo // 3

	# Reflect.has(obj, name)
		var myObject = {
		  foo: 1,
		};

		// 旧写法
		'foo' in myObject // true

		// 新写法
		Reflect.has(myObject, 'foo') // true

	# Reflect.deleteProperty(obj, name) 
		const myObj = { foo: 'bar' };

		// 旧写法
		delete myObj.foo;

		// 新写法
		Reflect.deleteProperty(myObj, 'foo');

	# Reflect.construct(target, args)
		function Greeting(name) {
		  this.name = name;
		}

		// new 的写法
		const instance = new Greeting('张三');

		// Reflect.construct 的写法
		const instance = Reflect.construct(Greeting, ['张三']);

	# Reflect.getPrototypeOf(obj)
		const myObj = new FancyThing();

		// 旧写法
		Object.getPrototypeOf(myObj) === FancyThing.prototype;

		// 新写法
		Reflect.getPrototypeOf(myObj) === FancyThing.prototype;

	# Reflect.setPrototypeOf(obj, newProto)
		const myObj = new FancyThing();

		// 旧写法
		Object.setPrototypeOf(myObj, OtherThing.prototype);

		// 新写法
		Reflect.setPrototypeOf(myObj, OtherThing.prototype);

	# Reflect.apply(func, thisArg, args)
		const ages = [11, 33, 12, 54, 18, 96];

		// 旧写法
		const youngest = Math.min.apply(Math, ages);
		const oldest = Math.max.apply(Math, ages);
		const type = Object.prototype.toString.call(youngest);

		// 新写法
		const youngest = Reflect.apply(Math.min, Math, ages);
		const oldest = Reflect.apply(Math.max, Math, ages);
		const type = Reflect.apply(Object.prototype.toString, youngest, []);

	# Reflect.defineProperty(target, propertyKey, attributes)
		function MyDate() {
		  /*…*/
		}

		// 旧写法
		Object.defineProperty(MyDate, 'now', {
		  value: () => new Date.now()
		});

		// 新写法
		Reflect.defineProperty(MyDate, 'now', {
		  value: () => new Date.now()
		});
	
	# Reflect.getOwnPropertyDescriptor(target, propertyKey)
		var myObject = {};
		Object.defineProperty(myObject, 'hidden', {
		  value: true,
		  enumerable: false,
		});

		// 旧写法
		var theDescriptor = Object.getOwnPropertyDescriptor(myObject, 'hidden');

		// 新写法
		var theDescriptor = Reflect.getOwnPropertyDescriptor(myObject, 'hidden');

	# Reflect.ownKeys (target)
		var myObject = {
		  foo: 1,
		  bar: 2,
		  [Symbol.for('baz')]: 3,
		  [Symbol.for('bing')]: 4,
		};

		// 旧写法
		Object.getOwnPropertyNames(myObject)
		// ['foo', 'bar']

		Object.getOwnPropertySymbols(myObject)
		//[Symbol.for('baz'), Symbol.for('bing')]

		// 新写法
		Reflect.ownKeys(myObject)
		// ['foo', 'bar', Symbol.for('baz'), Symbol.for('bing')]
	
	# 使用 Proxy 实现观察者模式

		const person = observable({
		  name: '张三',
		  age: 20
		});

		function print() {
		  console.log(`${person.name}, ${person.age}`)
		}

		observe(print);
		person.name = '李四';
		// 输出
		// 李四, 20

		实现
		const queuedObservers = new Set();
		const observe = fn => queuedObservers.add(fn);
		const observable = obj => new Proxy(obj, {set});

		function set(target, key, value, receiver) {
		  const result = Reflect.set(target, key, value, receiver);
		  queuedObservers.forEach(observer => observer());
		  return result;
		}


