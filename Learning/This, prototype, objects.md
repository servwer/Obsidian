`this` - привязка, которая создается при вызове функции, на что она ссылается определеяется где и при каких условиях функция была вызвана.

Точка вызова - место в коде где была вызвана функция

 # Правила привязки `this`
**Привязка по умолчанию**

```js
	function foo() {
		console.log( this.a );
	}

	var a = 2;

	foo(); // 2
```

1. Все глобальные переменные являются синонимами глобальных свойств-объектов.
2. Когда вызывается `foo()` `this.a` указывает на глобальную переменную, срабатывает привязка по умолчанию. Это происходит из-за того, что наша функция вызывается по прямой необернутой ссылке на  функции
3. Это не будет работать если будет включен `strict mode` теле функции, тк `global` не попадает под действия привязки по умолчанию => `this` становится `undefined`

**Неявная привязка**
```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

obj.foo(); // 2
```
1. `foo()` была объявлена как значение параметра => она имеет доступ к контексту объекта `obj` и `this.a === obj.a`. Но `foo()` не содержится/не пренадлежит этому объекту как бы не была объявлена
2. Только верхний/последний уровень ссылки на свойство объекта имеет значения для точки вызова: 
   ```js
	function foo() {
		console.log( this.a );
	}

	var obj2 = {
		a: 42,
		foo: foo
	};
	
	var obj1 = {
		a: 2,
		obj2: obj2
	};
	
	obj1.obj2.foo(); // 42
   ```

**Неявно потерянный**
```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

var bar = obj.foo; // ссылка на функцию!!!!!!!!! равносильно var bar = foo;

var a = 35; // `a` также и свойство глобального объекта

bar(); // 35
```
Переменной `bar` присвоили значение `obj.foo`, на деле это является просто ссылкой на фунцию `foo`, из-за чего теряется привзяка к `obj` и здесь уже работает правило привязки по умолчанию.

```js
function foo() {
	console.log( this.a );
}

function doFoo(fn) {
	// `fn` — просто еще одна ссылка на `foo`

	fn(); // <-- точка вызова!
}

var obj = {
	a: 2,
	foo: foo
};

var a = 35; // `a` еще и переменная в глобальном объекте

doFoo( obj.foo ); // 35
```

Параметр  `fn` является неявным присвоением => опять теряется привязка к `obj` и действует правило привязки по умолчанию.
Тот же результат будет и если использовать функции языка, обработчики событий (ссылаются на DOM-элементы)

**Явная привязка**

Явную привязку можно реализовать используя функции `call(..)` и `apply(..)`
```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

foo.call( obj ); // 2
```
Вызов функции посредством `call()`, позволяет указать, что  `this` будет `obj`
Если в качестве параметра передавать примитивное значение (string, number, boolean), то это значение будет обернуто в свою объектную форму (new String...)

**Жесткая привязка**

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

var bar = function() {
	foo.call( obj );
};

bar(); // 2
setTimeout( bar, 100 ); // 2

// `bar` жестко привязывает `this` в `foo` к `obj`
// поэтому его нельзя перекрыть
bar.call( window ); // 2
```

Вызывая `bar` мы вручную привязываем вызываем `foo.call(obj)` что позволяет принудительно вызвать `foo` с привязкой `obj`, из-за чего мы мы не потеряем контекст, тк вызов `foo` всегда осуществляется через `bar` (если конечно это делать) 
Для **жесткой привязки** можно использовать `bind(..)` который возвращает новую функцию в которой жестко задан вызов оригинальной функии с переданным контекстом

**Привязка new**

Конструктор - функции которые связаны с операцией `new`

Когда функция создается и использованием операции `new` происходит следующее:
- Создается новый объект
- Объект связывается с прототипом
- Объект устанавливается как привязка `this` для вызова функции

```js
function foo(a) {
	this.a = a;
}

var bar = new foo( 2 );
console.log( bar.a ); // 2
```

# Контексты в вызовах API
Функции во многих библиотеках предоставляет необязательный параметр который является передаваем контекстом, это служит для того, чтобы обойтись использованием `bind`

```js
function foo(el) {
	console.log( el, this.id );
}

var obj = {
	id: "awesome"
};

// используем `obj` как `this` для вызовов `foo(..)`
[1, 2, 3].forEach( foo, obj ); // 1 awesome  2 awesome  3 awesome
```