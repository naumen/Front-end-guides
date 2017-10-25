# Руководство по стилю кода JavaScript

Другие руководства

- [React]

## Содержание <a name="table-of-contents"></a>

0. [Общие положение](#general)
1. [Типы](#types)
1. [Ссылки](#references)
1. [Объекты](#objects)
1. [Массивы](#arrays)
1. [Деструктурирующее присваивание](#destructuring)
1. [Строки](#strings)
1. [Функции](#functions)
1. [Стрелочные функции](#arrow-functions)
1. [Классы и конструкторы](#constructors)
1. [Модули](#modules)
1. [Итераторы и генераторы](#iterators-and-generators)
1. [Свойства](#properties)
1. [Переменные](#variables)
1. [Подъем](#hoisting)
1. [Операторы сравнения и равенство](#comparison)
1. [Блоки](#blocks)
1. [Управление выполнением]
1. [Комментарии]

## Общие положения <a name="general"></a>

**[К содержанию](#table-of-contents)**

## Типы <a name="types"></a>

<a name="types--primitives"></a><a name="1.1"></a>
- [1.1](#types--primitives) **Примитивы**: когда вы работаете с примитивом, вы работаете напрямую с его значением

  - `string`
  - `number`
  - `boolean`
  - `null`
  - `undefined`
  - `symbol`

  ```javascript
  const foo = 1;
  let bar = foo;

  bar = 9;

  console.log(foo, bar); // 1, 9
  ```

<a name="types--complex"></a><a name="1.2"></a>
- [1.2](#types--complex) **Сложные типы**: когда вы работаете со сложными типами, вы работаете со ссылкой на его значение

  - `object`
  - `array`
  - `function`

  ```javascript
  const foo = [1, 2];
  const bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // 9, 9
  ```

**[К содержанию](#table-of-contents)**

## Ссылки <a name="references"></a>

<a name="references--prefer-const"></a><a name="2.1"></a>
- [2.1](#references--prefer-const) Для всех ссылок используйте `const`, избегайте использования `var`

  eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

  >Почему: страховка от переопределения ссылок

  ```javascript
  // плохо
  var a = 1;
  var b = 2;

  // хорошо
  const a = 1;
  const b = 2;
  ```

<a name="references--disallow-var"></a><a name="2.2"></a>
- [2.2](#references--disallow-var) Если необходимо переопределять ссылки, используйте `let` вместо `var`

  eslint: [`no-var`](https://eslint.org/docs/rules/no-var)

  >Почему: у `let` уже область видимости - блок, вместо функции у `var`

  ```javascript
  // плохо
  var count = 1;
  if (true) {
    count += 1;
  }

  // хорошо
  let count = 1;
  if (true) {
    count += 1;
  }
  ```

<a name="references--block-scope"></a><a name="2.3"></a>
- [2.3](#references--block-scope) Обратите внимание, что область видимости `let` и `const` - блок

  ```javascript
  // const и let существуют только в блоках, в которых они объявлены
  {
    let a = 1;
    const b = 1;
  }
  console.log(a); // ReferenceError
  console.log(b); // ReferenceError
  ```

**[К содержанию](#table-of-contents)**

## Объекты <a name="objects"></a> [Черновик, необходимо обсуждение]

<a name="objects--no-new"></a><a name="3.1"></a>
- [3.1](#objects--no-new) Для объявления объекта используйте литерал

  eslint: [`no-new-objects`](http://eslint.org/docs/rules/no-new-objects.html)

  ```javascript
  // плохо
  const item = new Object();

  // хорошо
  const item = {};
  ```

<a name="objects--es6-computed-properties"></a><a name="3.2"></a>
- [3.2](#objects--es6-computed-properties) При создании объектов с динамическими именами свойств используйте вычисляемые имена свойств

  >Почему: использование вычисляемых имен свойств позволяет определить все свойства объекта в одном месте

  ```javascript
  function getKey (k) {
    return `a key named ${k}`;
  }

  // плохо
  const obj = {
    id: 5,
    name: 'San Francisco'
  };
  obj[getKey('enabled')] = true;

  // хорошо
  const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true
  };
  ```

<a name="objects--es6-object-shorthand"></a><a name="3.3"></a>
- [3.3](#objects--es6-object-shorthand) Используйте сохращенный синтаксис методов

  eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

  ```javascript
  // плохо
  const atom = {
    value: 1,
    addValue: function (value) {
      return atom.value + value;
    }
  };
  // хорошо
  const atom = {
    value: 1,
    addValue (value) {
      return atom.value + value;
    }
  };
  ```

<a name="objects--es6-object-concise"></a><a name="3.4"></a>
- [3.4](#objects--es6-object-concise) Используйте сокращенный синтаксис свойств

  eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

  >Почему: такая запись кратка и описательна

  ```javascript
  // плохо
  const obj = {
    nerevar: nerevar
  };

  // хорошо
  const obj = {
    nerevar
  };
  ```

<a name="objects--grouped-shorthand"></a><a name="3.5"></a>
- [3.5](#objects--grouped-shorthand) Группируйте сокращенные свойства в начале объявления объекта

  >Почему: так легче определить какие свойства используют сокращенный синтаксис

  ```javascript
  const almalexia = 'Almalexia';
  const sothasil = 'Sotha Sil';
  const vivec = 'Vivec';

  // плохо
  const obj = {
    arena: 1,
    almalexia,
    daggerall: 2,
    sothasil,
    morrowind: 3,
    vivec
  };

  // хорошо
  const obj = {
    almalexia,
    sothasil,
    vivec,
    arena: 1,
    daggerall: 2,
    morrowind: 3
  };
  ```

<a name="objects--quoted-props"></a><a name="3.6"></a>
- [3.6](#objects--quoted-props) Заключайте в кавычки только те свойства, имена которых не являются корректными идентификаторами

  eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)

  >Почему: Субъективно легче читать; улучшает подсветку синтаксиса; улучшает оптимизацию разными JS-движками

  ```javascript
  // плохо
  const obj = {
    'foo': 1,
    'bar': 2,
    'data-baz': 3
  };

  // хорошо
  const obj = {
    foo: 1,
    bar: 2,
    'data-baz': 3
  };
  ```

<a name="objects--prototype-builtins"></a><a name="3.7"></a>
- [3.7](#objects--prototype-builtins) Не вызывайте напрямую методы `Object.prototype` (например, `hasOwnProperty`, `propertyIsEnumerable`, `isPrototypeOf`)

  >Почему: эти свойства могут быть переопределены (`{hasOwnProperties: false}`) или объект может быть нулевым (`Object.create(null)`)

  ```javascript
  // плохо
  console.log(object.hasOwnProperty(key));

  // хорошо
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // наилучший способ
  const has = Object.prototype.hasOwnProperty; // кеш
  // или
  import has from 'has';
  // ...
  console.log(has.call(object, key));
  ```

<a name="objects--rest-spread"></a><a name="3.8"></a>
- [3.8](#objects--rest-spread) Используйте `spread`-оператор вместо [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) для поверхностного копирования объектов; используйте `rest`-оператор для создания объекта с оставшимися свойствами исходного

  ```javascript
  // очень плохо
  const original = {a: 1, b: 2};
  const copy = Object.assign(original, {c: 3}); // этот код меняет original
  delete copy.a; // этот - тоже

  // плохо
  const original = {a: 1, b: 2};
  const copy = Object.assign({}, original, {c: 3}); // copy => {a: 1, b: 2, c: 3}

  // хорошо
  const original = {a: 1, b: 2};
  const copy = {...original, c: 3}; // copy => {a: 1, b: 2, c: 3}

  const {a, ...noA} = copy; // noA => {b: 2, c: 3}
  ```

**[К содержанию](#table-of-contents)**

## Массивы <a name="arrays"></a> [Черновик, необходимо обсуждение]

<a name="arrays--literals"></a><a name="4.1"></a>
- [4.1](#arrays--literals) Для объявления массива используйте литерал

  eslint: [`no-array-constructor`](http://eslint.org/docus/rules/no-array-constructor.html)

  ```javascript
  // плохо
  const items = new Array();

  // хорошо
  const items = [];
  ```

<a name="arrays--push"></a><a name="4.2"></a>
- [4.2](#arrays--push) Используйте [Array#push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push?v=b) вместо прямого присваивания значений элементам массива

  ```javascript
  const someStack = [];

  // плохо
  someStack[someStack.length] = 'some value';

  // хорошо
  someStack.push('some value');
  ```

<a name="arrays--es6-array-spreads"></a><a name="4.3"></a>
- [4.3](#arrays--es6-array-spreads) Для копирования массивов используйте `spread`-оператор (`...`)

  ```javascript
  // плохо
  const len = items.length;
  const itemsCopy = [];
  let i;

  for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
  }

  // хорошо
  const itemsCopy = [...items];
  ```

<a name="arrays--from"></a><a name="4.4"></a>
- [4.4](#arrays--from) Чтобы сконвертировать массивоподобный объект в массив используйте `spread`-оператор (`...`) вместо [Array#from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

  ```javascript
  const foo = document.querySelectorAll('.foo');
  // плохо
  const nodes = Array.from(foo);

  // хорошо
  const nodes = [...foo];

<a name="arrays--mapping"></a><a name="4.5"></a>
- [4.5](#arrays--mapping) Испорльзуйте [Array#from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) вместо `spread`-оператора (`...`) при маппинге по итерируемым типам

  >Почему: при таком подходе не создается промежуточный массив

  ```javascript
  // плохо
  const baz = [...foo].map(bar);

  // хорошо
  const baz = Array.from(foo, bar);
  ```

<a name="arrays--callback-return"></a><a name="4.6"></a>
- [4.6](#arrays--calback-return) Используйте `return` в функции обратного вызова метода массива; если тело функции состоит из одной строки, возвращающей выражение без побочных эффектов, `return` можно опустить (см. [8.2](#arrow-functions--implicit-return))

  eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

  ```javascript
  // хорошо
  [1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
  });

  // хорошо
  [1, 2, 3].map(x => x + 1);

  // плохо - отсутствие возвращаемого значения приведет к тому, что memo будет undefined после первой итерации
  const flat = {};
  [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item);
    memo[index] = flatten;
  });

  // хорошо
  const flat = {};
  [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item);
    memo[index] = flatten;
    return flatten;
  });

  // плохо
  inbox.filter(msg => {
    const {author, subject} = msg;
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee'
    } else {
      return false;
    }
  });

  // хорошо
  inbox.filter(msg => {
    const {author, subject} = msg;
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee'
    }

    return false;
  });
  ```

<a name="arrays--bracket-newline"></a><a name="4.7"></a>
- [4.7](#arrays--bracket-newline) В многострочном массиве используйте переводы строк после открывающей скобки и перед закрывающей скобкой

  ```javascript
  // плохо
  const arr = [
    [0, 1], [2, 3], [4, 5]
  ];

  const objectInArray = [{
    id: 1
  }, {
    id : 2
  }];

  const numberInArray = [
    1, 2
  ];

  // хорошо
  const arr = [[0, 1], [2, 3], [4, 5]];

  const objectInArray = [
    {
      id: 1
    },
    {
      id : 2
    }
  ];

  const numberInArray = [
    1,
    2
  ];
  ```

## Деструктурирующее присваивание <a name="destructuring"></a>

<a name="destructuring--object"></a><a name="5.1"></a>
- [5.1](#destructuring--object) При необходимости использования нескольких свойств объекта используйте деструктурирующее присваивание

  >Почему: не создаются временные ссылки на эти свойства

  ```javascript
  // плохо
  function getFullName (user) {
    const firstName = user.firstName;
    const lastName = user.lastName;

    return `${firstName} ${lastName}`;
  }

  // хорошо
  function getFullName (user) {
    const {firstName, lastName} = user;

    return `${firstName} ${lastName}`;
  }

  // наилучший способ
  function getFullName ({firstName, lastName}) {
    return `${firstName} ${lastName}`;
  }
  ```

<a name="destructuring--array"></a><a name="5.2"></a>
- [5.2](#destructuring--array) Используйте деструктурирующее присваивание при работе с массивами

  ```javascript
  const arr = [1, 2, 3];

  // плохо
  const first = arr[0];
  const second = arr[1];

  // хорошо
  const [first, second] = arr;
  ```

<a name="destructuring--object-over-array"></a><a name="5.3"></a>
- [5.3](#destructuring--object-over-array) Используйте деструктурирование объекта, если необходимо вернуть несколько значений

  >Почему: со временем можно добавить или удалить возвращаемые значения или поменять их порядок не боясь сломать код в месте использования

  ```javascript
  // плохо
  function processToken(token) {
    // ...
    return [location, roles, token, user];
  }

  // при использовании функции необходимо помнить порядок
  const [location, __, user] = processToken(token);

  // хорошо
  function processToken(token) {
    // ...
    return {location, roles, token, user};
  }

  // при использовании берется только нужное
  const {location, user} = processToken(token);
  ```

**[К содержанию](#table-of-contents)**

## Строки <a name="strings"></a>

<a name="strings--quotes"></a><a name="6.1"></a>
- [6.1](#strings--quotes) Используйте одинарные кавычки (`''`) для строк

  eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html)

  ```javascript
  // плохо
  const name = "Перциус Мерциус";

  // плохо
  const name = `Перциус Мерциус`;

  // хорошо
  const name = 'Перциус Мерциус';
  ```

<a name="strings--line-length"></a><a name="6.2"></a>
- [6.2](#strings--line-length) Не переносите длинный текст

  >Почему: с перенесенными строками сложнее работать и текст сложнее найти используя функцию поиска IDE

  ```javascript
  // плохо
  const errorMessage = 'Fetch failed while trying to get new shiny just out of foundry token \
    for user with some weird login at Service Desk. The original error mesage was to long to \
    log it in browser console';

  // плохо
  const errorMessage = 'Fetch failed while trying to get new shiny just out of foundry token' +
    'for user with some weird login at Service Desk. The original error mesage was to long to ' +
    'log it in browser console';

  // хорошо
  const errorMessage = 'Fetch failed while trying to get new shiny just out of foundry token for user with some weird login at Service Desk. The original error mesage was to long to log it in browser console';
  ```

<a name="strings--es6-template-literals"></a><a name="6.3"></a>
- [6.3](#strings--es6-template-literals) При программной генерации строк используйте строковые шаблоны вместо конкатенации

  eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html), [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing)

  >Почему: строковые шаблоны обладают читаемым, кратким синтаксисом с поддержкой новых строк и интерполяцией

  ```javascript
  // плохо
  function sayHi (name) {
    return 'Hi, ' + name;
  }

  // плохо
  function sayHi (name) {
    return ['Hi, ', name].join();
  }

  // плохо
  function sayHi (name) {
    return `Hi, ${ name }`;
  }

  // хорошо
  function sayHi (name) {
    return `Hi, ${name}`;
  }
  ```

<a name="strings--eval"></a><a name="6.4"></a>
- [6.4](#strings--eval) Никогда не используейте `eval()` на строках, это открывает слишком много уязвимостей

  eslint: [`no-eval`](http://eslint.org/docs/rules/no-eval)

<a name="strings--escaping"><a name="6.5"></a>
- [6.5](#strings-escaping) Не используйте экранирование символов без необходимости

  eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

  >Почему: экранирование ухудшает читаемость

  ```javascript
  // плохо
  const foo = '\'this\' \i\s \"quoted\"';

  // хорошо
  const foo = '\'this\' is "quoted"';
  const foo = `my name is '${name}'`;
  ```

**[К содержанию](#table-of-contents)**

## Функции <a name="functions"></a>

<a name="functions--signature-spacing"></a><a name="7.1"></a>
- [7.1](#functions--signature-spacing) Пробельные символы в сигнатуре функции

  eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren), [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks), [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing)
  
  >Почему: паттерн для поиска определения функции отличается от паттерна поиска ее использования. Разделение ключевых слов и редактируемого кода.

  ```javascript
  // плохо
  const l = function(){};
  const m = function (){};
  const n = function() {}; 

  // хорошо
  const x = function () {};
  const y = function a () {};
  ```

## Стрелочные функции <a name="arrow-functions"></a>

<a name="arrow-functions--use-them"></a><a name="8.1"></a>
- [8.1](#arrow-functions--use-them) При необходимости использовать функционально выражение (например, при передаче в качестве аргумента безымянной функции) используйте стрелочные функции

  eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html)

  >Почему: краткий синтаксис и захват контекста `this`

  >Почему нет: в сложных функциях лучше вынести логику в отдельную именованную функцию

  ```javascript
  // плохо
  [1, 2, 3].map(function (x) {
    const y = x + 1;
    return x * y;
  });

  // хорошо
  [1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
  });
  ```

<a name="arrow-functions--implicit-return"></a><a name="8.2"></a>
- [8.2](#arrow-functions--implicit-return) Если тело функции состоит из одной строки, которая возвращает [выражение](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) без побочных эффектов, опустите фигурные скобки и используйте неявный `return`

  eslint: [`arrow-parens`](http://esling.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.og/docs/rules/arrow-body-style.html)

  >Почему: легко читается, когда несколько функций образуют цепочку

  ```javascript
  // плохо
  [1, 2, 3].map(number => {
    const nextNumber = number + 1;
    `A string containing the ${nextNumber}`;
  });

  // хорошо
  [1, 2, 3].map(number => `A string containing the ${number}`);

  // хорошо
  [1, 2, 3].map(number => {
    const nextNumber = number + 1;
    return `A string containing the ${nextNumber}`;
  });

  // хорошо
  [1, 2, 3].map((number, index) => ({
    [index]: number
  }));

  // без неявного return, с побочными эффектами
  function foo (callback) {
    const val = callback();
    if (val === true) {
      // ...
    }
  }

  let bool = false;

  // плохо
  foo(() => bool = true);

  // хорошо
  foo(() => {
    bool = true;
  });
  ```

<a name="arrow-functions--paren-wrap"></a><a name="8.3"></a>
- [8.3](#arrow-functions--paren-wrap) В случае, когда выражение занимает несколько строк, оберните его в скобки для лучшей читаемости

  >Почему: скобки хорошо обозначают начало и конец функции

  ```javascript
  // плохо
  ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
      httpMagicObjectWithAVeryLongName,
      httpMethod
    )
  );

  // хорошо
  ['get', 'post', 'put'].map(httpMethod => (
    Object.prototype.hasOwnProperty.call(
      httpMagicObjectWithAVeryLongName,
      httpMethod
    )
  ));
  ```
<a name="arrow-functions--one-arg-parens"></a><a name="8.4"></a>
- [8.4](#arrow-functions--one-arg-parens) Если функция принимает один аргумент, не используйте скобки. Если необходимо указать тип аргумента, скобки используются.

  >Почему: более чистый код

  ```javascript
  // плохо
  [1, 2, 3].map((x) => x * x);

  // хорошо
  [1, 2, 3].map(x => x * x);

  // хорошо
  [1, 2, 3].map(number=> (
    `A long string with ${number}. It's so long that we don't want it to take up space on the .map line!`
  ));
  ```

<a name="arrow-functions--confusing"></a><a name="8.5"></a>
- [8.5](#arrow-functions--confusing) Избегайте использования синтаксиса стрелочных функций (`=>`) с синтаксисом оперторов сравнения (`<=`, `=>`)

  eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

  ```javascript
  // плохо
  const itemHeight = item => itemHeight > 256 ? item.largeSize : item.smallSize;

  // плохо
  const itemHeight = (item) => itemHeight > 256 ? item.largeSize : item.smallSize;

  // хорошо
  const itemHeight = item => (itemHeight > 256 ? item.largeSize : item.smallSize);

  // хорошо
  const itemHeight = item => {
    const {height, largeHeight, smallHeight} = item;
    return height > 256 ? largeSize : smallSize;
  };
  ```

**[К содержанию](#table-of-contents)**

## Классы и конструкторы <a name="constructors"></a>

<a name="constructors--use-class"></a><a name="9.1"></a>
- [9.1](#constructors--use-class) Всегда используйте `class`, избегайте прямого изменения `prototype`

  >Почему: синтаксис классов более краток и прост для понимания

  ```javascript
  // плохо
  function Queue (contents = []) {
    this.queue = [...contents];
  }
  Queue.prototype.pop = function () {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  };

  // хорошо
  class Queue {
    constructor (contents = []) {
      this.queue = [...contents];
    }
    pop () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    }
  };
  ```

<a name="constructors--extends"></a><a name="9.2"></a>
- [9.2](#constructors--extends) Используйте `extends` для наследования

  >Почему: это встроенный способ наследования финкциональности прототипа без необходимости ломать `instanceof`

  ```javascript
  // плохо
  const inherits = require('inherits');
  function PeekableQueue (contents) {
    Queue.apply(this, contents);
  }
  inherits(PeekableQueue, Queue);
  PeekableQueue.prototype.peek = function () {
    return this.queue[0];
  };

  // хорошо
  class PeekableQueue extends Queue {
    peek () {
      return this.queue[0];
    }
  }
  ```

<a name="constructors--chaining"></a><a name="9.3"></a>
- [9.3](#constructors--chaining) Методы могут возвращать `this` для обеспечения чейнинга

  ```javascript
  // плохо
  Telvanni.prototype.fly = function () {
    this.flying = true;
    return true;
  };

  Telvanni.prototype.setHeight = function (height) {
    this.height = height;
  };

  const mage = new Telvanni();

  mage.fly(); // => true
  mage.setHeight(20); // undefined

  // хорошо
  class Telvanni {
    fly () {
      this.flying = true;
      return this;
    }
  
    setHeight (height) {
      this.height = height;
      return this;
    }
  }

  const mage = new Telvanni();

  mage.fly()
    .setHeight(20);
  ```

<a name="constructors--tostring"></a><a name="9.4"></a>
- [9.4](#constructors--tostring) Ничего плохого в написании своих методов `toString`, если вы убедились, что они работают нормально и не имеют побочных эффектов

  ```javascript
  class Redoran {
    constructor (options = {}) {
      this.name = options.name || 'no name';
    }

    getName () {
      return this.name;
    }

    toString () {
      return `Redoran - ${this.getName()}`;
    }
  }
  ```

<a name="constructors--no-useless"></a><a name="9.5"></a>
- [9.5](#constructors--no-useless) У классов есть конструктор по-умолчанию, если не указан собственный конструктор; пустой конструктор или конструктор, только вызывающий родительский конструктор бесполезен

  eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

  ```javascript
  // плохо
  class Redoran {
    constructor () {}

    getName () {
      return this.name;
    }
  }

  // плохо
  class Warrior extends Redoran {
    constructor (...args) {
      super(...args);
    }
  }

  // хорошо
  class Warrior extends Redoran {
    constructor (...args) {
      super(...args);
      this.name = 'Nameless Hero';
    }
  }
  ```

<a name="classes--no-duplicate-members"></a><a name="9.6"></a>
- [9.6](#classes--no-duplicate-members) Избегайте повторов членов класса

  eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

  >Почему: при дублировании членов класса использоваться будет последний, ошибки при этом не будет; в большинстве случаев дублирование - ошибка

  ```javascript
  // плохо
  class Foo {
    bar () { return 1; }
    bar () { return 2; }
  }

  // хорошо
  class Foo {
    bar () { return 1; }
  }

  // хорошо
  class Foo {
    bar () { return 2; }
  }
  ```

**[К содержанию](#table-of-contents)**

## Модули <a name="modules"></a>

<a name="modules--use-them"></a><a name="10.1"></a>
- [10.1](#modules--use-them) Всегда используйте модули (`import`/`export`) вместо нестандартной модульной системы. Вы всегда сможете перевести код в предпочитаемую модульную систему.

  >Почему: за модулями будущее, чем раньше начнем их использовать, тем лучше

  ```javascript
  // плохо
  const SuperModule = require('./SuperModule');
  module.exports = SuperModule.AwesomeFeature;

  // нормально
  import SuperModule from './SuperModule';
  export default SuperModule.AwesomeFeature;

  // отлично
  import {AwesomeFeature} from './SuperModule';
  export default AwesomeFeature;
  ```

<a name="modules--no-wildcard"></a><a name="10.2"></a>
- [10.2](#modules--no-wildcard) Не используйте «звездочку» при импорте

  >Почему: это дает уверенность, что есть только один экспорт по-умолчанию

  ```javascript
  // плохо
  import * as SuperModule from './SuperModule';

  // хорошо
  import SuperModule from './SuperModule';
  ```

<a name="modules--no-export-from-import"></a><a name="10.3"></a>
- [10.3](#modules--no-export-from-import) Не эспортируйте напрямую из импорта

  >Почему: хотя одна строка и короче, один чистый импорт и один чистый экспорт консистентны

  ```javascript
  // плохо
  // файл AwesomeFeature.js
  export {AwesomeFeature as default} from './SuperModule';

  // хорошо
  // файл AwesomeFeature.js
  import {AwesomeFeature} from './SuperModule';
  export default AwesomeFeature;
  ```

<a name="modules--no-duplicate-imports"></a><a name="10.4"></a>
- [10.4](#modules--no-duplicate-imports) Единственный импорт для каждого источника

  eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)

  >Почему: код, в котором есть несколько строк импорта из одного и того же источника, тяжелее поддерживать

  ```javascript
  // плохо
  import foo from 'foo';
  // другие импорты
  import {named1, named2} from 'foo';

  // хорошо
  import foo, {named1, named2} from 'foo';
  ```

<a name="modules--no-mutable-exports"></a><a name="10.5"></a>
- [10.5](#modules--no-mutable-exports) Экспортируйте только константы

  eslint: [`import/no-mutable-exports`](http://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

  >Почему: в целом, стоит избегать изменений значений (mutations). В частности - стоит экспортировать только константы. Это обеспечивает предсказуемость результата.

  ```javascript
  // плохо
  let foo = 'bar';
  export {foo};

  // хорошо
  const foo = 'bar';
  export {foo};
  ```

<a name="modules--prefer-default-export"></a><a name="10.6"></a>
- [10.6](#modules--prefer-default-export) Если в модуле только один экспорт, предпочитайте экспорт по умолчанию именованному экспорту

  eslint: [`import/prefer-default-export`](http://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

  >Почему: улучшает читаемость и понимаемость кода

  ```javascript
  // плохо
  export function foo () {};

  // хорошо
  export default function foo () {};
  ```

<a name="modules--imports-first"></a><a name="10.7"></a>
- [10.7](#modules--imports-first) Помещайте строки импорта выше строк кода

  eslint: [`import/first`](http://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

  >Почему: импорты поднимаются (hoist), размещение их в начале предупреждает неожиданное поведение

  ```javascript
  // плохо
  import foo from 'foo';
  foo.init();

  import bar from 'bar';

  // хорошо
  import bar from 'bar';
  import foo from 'foo';

  foo.init();
  ```

<a name="modules--multiline-imports-over-newlines"></a><a name="10.8"></a>
- [10.8](#modules--multiline-imports-over-newlines) Многострочные импорты должны форматироваться так же как многострочные массивы или литералы объектов

  >Почему: фигурные скобки импорта следуют тому же стилю форматирования, что и любые другие фигурные скобки блоков (равно как и запятые)

  ```javascript
  // плохо
  import {longName1, longName2, longName3, longName4, longName5, longName6, longName7,
    longName8, longName9} from 'module';

  // хорошо
  import {
    longName1,
    longName2,
    longName3,
    longName4,
    longName5,
    longName6,
    longName7,
    longName8,
    longName9
  } from 'module';
  ```

<a name="modules--no-webpack-loader-syntax"></a><a name="10.9"></a>
- [10.9](#modules--no-webpack-loader-syntax) Не указывайте загрузчики Webpack'а в импортах

  eslint: [`import/no-webpack-loader-syntax`](http://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

  >Почему: этот нестандартный синтаксис привязывает код к сборщику; для указания загрузчиков стоит использовать `webpack.config.js`

  ```javascript
  // плохо
  import fooLess from 'css!less!foo.less';
  import barCss from 'style!css!bar.css';

  // хорошо
  import fooLess from 'foo.less';
  import barCss from 'bar.css';
  ```

**[К содержанию](#table-of-contents)**

## Итераторы и генераторы <a name="iterators-and-generators"></a>

<a name="iterators--nope"></a><a name="11.1"></a>
- [11.1](#iterators--nope) Не используйте итераторы, предпочитайте функции высшего порядка циклам типа `for-in` и `for-of`

  eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator), [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

  >Почему: для усиления правила о неизменяемости: с чистыми функциями, которые возращают значение, легче работать чем с побочными эффектами

  >Используйте `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` для перебора массивов и `Object.keys()` / `Object.values()` / `Object.entries()` для создания массивов с целью перебора объекта

  ```javascript
  const numbers = [1, 2, 3, 4, 5];

  // плохо
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  sum === 15;

  // хорошо
  let sum = 0;
  numbers.forEach(num => sum += num);
  sum === 15;

  // наилучший способ
  const sum = numbers.reduce((total, num) => total + num, 0);
  sum === 15;

  // плохо
  const increasedByOne = [];
  for (let i = 0; i < numbers.length; i++) {
    increasedByOne.push(numbers[i] + 1);
  }

  // хорошо
  const increasedByOne = [];
  numbers.forEach(num => increasedByOne.push(num + 1));

  // наилучший способ
  const increasedByOne = numbers.map(num => num + 1);
  ```

<a name="generators--nope"></a><a name="11.2"></a>
- [11.2](#generators--nope) Не используйте генераторы пока

  >Почему: они не очень хорошо переводятся в ES5

<a name="generators--spacing"></a><a name="11.3"></a>
- [11.3](#generators-spacing) Если использование генераторов - необходимость, используйте пробельные символы в сигнатуре функции правильно

  eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

  >Почему: `function` и `*` - две части одного концептуального ключевого слова: `*` не модификатор `function`, `function*` - уникальная конструкция, отличная от `function`

  ```javascript
  // плохо
  function * foo () {
    // ...
  }

  // плохо
  const bar = function * () {
    // ...
  };

  // плохо
  const baz = function *() {
    // ...
  };

  // плохо
  const quux = function*() {
    // ...
  };

  // плохо
  function*foo () {
    // ...
  }

  // плохо
  function *foo () {
    // ...
  }

  // очень плохо
  function
  *
  foo() {
    // ...
  }

  // очень плохо
  const wat = function
  *
  () {
    // ...
  };

  // хорошо
  function* foo () {
    // ...
  }

  // хорошо
  const foo = function* () {
    // ...
  };
  ```

**[К содержанию](#table-of-contents)**

## Свойства <a name="#properties"></a>

<a name="properties--dot"></a><a name="12.1"></a>
- [12.1](#properties--dot) Используйте «точку» при получении доступа к свойству

  eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation)

  ```javascript
  const kogoruhn = {
    fortress: true
  };

  // плохо
  const isFortress = kogoruhn['fortress'];

  // хорошо
  const isFortress = kogoruhn.fortress;
  ```

<a name="properties--bracket"></a><a name="12.2"></a>
- [12.2](#properties--bracket) Используйте квадратные скобки (`[]`) при получении доступа к свойству с использованием переменной

  ```javascript
  const kogoruhn = {
    fortress: true
  };

  function getProp (prop) {
    return kogoruhn[prop];
  }

  const isFortress = getProp('fortress');
  ```

<a name="properties--es2016-exponentiation-operator"></a><a name="12.3"></a>
- [12.3](#properties--es2016-expoenentiation-operator) Используйте оператор возведения в степень `**` при подсчете степени

  eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties)

  ```javascript
  // плохо
  const binary = Math.pow(2, 10);

  // хорошо
  const binary = 2 ** 10;
  ```

**[К содержанию](#table-of-contents)**

## Переменные <a name="variables"></a> [Черновик, необходимо обсуждение]

<a name="variables--const"></a><a name="1.1"></a>
- [13.1](#variables--const) При объявлении переменных всегда используйте `const` или `let`

  eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef), [`prefer-const`](http://eslint.org/docs/rules/prefer-const)

  >Почему: использование `var` приводит к глобальным переменным и замусориванию глобальной области видимости

  ```javascript
  // плохо
  var persons = ['Almalexia', 'Sotha Sil', 'Vivec'];

  // хорошо
  const persons = ['Almalexia', 'Sotha Sil', 'Vivec'];
  ```

<a name="variables--one-const"></a><a name="13.2"></a>
- [13.2](#variables--one-const) Используйте `const` или `let` для объявления одной переменной

  eslint: [`one-var`](https://eslint.org/docs/rules/one-var)

  >Почему: так легче добавлять объявления переменных; не придется беспокоиться о замене `;` на `,`; можно пройтись отладчиком по каждой переменной

  ```javascript
  // плохо
  const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

  // плохо
  // в таком объявлении легко допустить ошибку
  const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

  // хорошо
  const items = getItems();
  const goSportsTeam = true;
  const dragonball = 'z';
  ```

<a name="variables--const-let-group"></a><a name="13.3"></a>
- [13.3](#variables--const-let-group) Группируйте все объявления `const`, потом группируйте все объявления `let`

  >Почему: помогает при необходимости определить переменную на основе уже объявленных переменных; легче читается

  ```javascript
  // плохо
  let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

  // плохо
  let i;
  const items = getItems();
  let dragonball;
  const goSportsTeam = true;
  let len;

  // хорошо
  const goSportsTeam = true;
  const items = getItems();
  let dragonball;
  let i;
  let len;
  ```

<a name="variables--define-where-used"></a><a name="13.4"></a>
- [13.4](#variables--define-where-used) Присваивайте значения переменным там, где вы их используете, но помещайте их в разумное место

  >Почему: область видимости `let` и `const` - блок, не функция

  ```javascript
  // плохо - вызов функции без необходимости
  function checkName (hasName) {
    const name = getName();

    if (hasName === 'test') {
      return false;
    }

    if (name === 'test') {
      this.setName('');
      return false;
    }

    return name;
  }

  // хорошо
  function checkName (hasName) {
    if (hasName === 'test') {
      return false;
    }

    const name = getName();

    if (name === 'test') {
      this.setName('');
      return false;
    }

    return name;
  }
  ```

<a name="variables--no-chain-assignment"></a><a name="13.5"></a>
- [13.5](#variables--no-chain-assignment) Не присваивайте значения по цепочке

  >Почему: использование присваивания по цепочке создает неявные глобальные переменные

  ```javascript
  // плохо
  (function example () {
    // JavasScript интерпретирует это как
    // let a = (b = (c = 1));
    // ключевое слово let применяется только к 'a'
    // b и c становятся глобальными переменными
    let a = b = c = 1;
  }());

  console.log(a); // выбрасывает ReferenceError
  console.log(b); // 1
  console.log(c); // 1

  // хорошо
  (function example () {
    let a = 1;
    let b = 1;
    let c = 1;
  }());

  console.log(a); // выбрасывает ReferenceError
  console.log(b); // выбрасывает ReferenceError
  console.log(c); // выбрасывает ReferenceError

  // то же самое относится и к `const`
  ```

<a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
- [13.6](#variables--unary-increment-decrement) Избегайте использования унарных операторов увеличения и уменьшения (++, --)

  eslint: [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

  >Почему: после унарных операторов необходимо автоматически вставлять точку с запятой; если этого не делать, то разница в использовании пробельных символов может привести к ошибке

  ```javascript
  // плохо
  const array = [1, 2, 3];
  let num = 1;
  num++;
  --num;

  let sum = 0;
  let truthyCount = 0;
  for (let i = 0; i < array.count; i++) {
    let value = array[i];
    sum += value;
    if (value) {
      truthyCount++;
    }
  }

  // хорошо
  const array = [1, 2, 3];
  let num = 1;
  num += 1;
  num -= 1;

  const sum = array.reduce((a, b) => a + b, 0);
  const truthyCount = array.filter(Boolean).length;
  ```

**[К содержанию](#table-of-contents)**

## Подъем <a name="hoisting"></a>

<a name="hoisting--about"></a><a name="14.1"></a>
- [14.1](#hoisting--about) Объявления `var` поднимаются к началу области видимости, присвоения им - нет; объявления `const` и `let` обладают временной мертвой зоной ([Temporal Dead Zone](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let)); также важно знать почему [typeof больше не безопасен](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)

  ```javascript
  // мы знаем, что это не будет работать
  // (предполагается, что notDefined нигде в коде не определен)
  function example () {
    console.log(notDefined); // ReferenceError
  }

  // если создать переменную после ее использования, то такой код будет работать
  // т.к. объявление переменной поднимается к началу области видимости;
  // при этом присвоение не поднимается
  function example () {
    console.log(declaredButNotAssigned); // undefined
    var declaredButNotAssigned = true;
  }

  // интерпретатор поднимает объявление переменной к началу области видимости;
  // код выше можно переписать так
  function example () {
    let declaredButNotAssigned;
    console.log(declaredButNotAssigned); // undefined
    declaredButNotAssigned = true;
  }

  // использование const и let
  function example () {
    console.log(declaredButNotAssigned); // ReferenceError
    console.log(typeof declaredButNotAssigned); // ReferenceError
    const declaredButNotAssigned = true;
  }
  ```

<a name="hoisting--anon-expressions"></a><a name="14.2"></a>
- [14.2](#hoisting--anon-expressions) Безымянные функциональные выражения поднимают название переменной, но не присвоение функции

  ```javascript
  function example () {
    console.log(anonymous); //undefined

    anonymous(); // TypeError (anonymous is not a function)

    var anonymous = function () {
      console.log('anonymous function expression');
    }
  }
  ```

<a name="hoisting--named-expressions"></a><a name="14.3"></a>
- [14.3](#hoisting--named-expressions) Именованные функциональные выражения поднимают название переменной, но не название и тело функции

  ```javascript
  function example () {
    console.log(named); // undefined

    named(); // TypeError (named is not a function)

    superPower(); // ReferenceError (superPower is not defined)

    var named = function superPower () {
      console.log('Flying');
    }
  }

  function example () {
    console.log(named); // undefined

    named(); // TypeError (named is not a function)

    var named = function named () {
      console.log('named');
    }
  }
  ```

<a name="hoisting--declarations"></a><a name="14.4"></a>
- [14.4](#hoisting--declarations) Объявления функций поднимают как имя, так и тело функции

  ```javascript
  function example () {
    superPower(); // Flying

    function superPower () {
      console.log('Flying');
    }
  }
  ```

## Операторы сравнения и равенство <a name="comparison"></a> [Черновик, необходимо обсуждение]

<a name="comparison--eqeqeq"></a><a name="15.1"></a>
- [15.1](#comparison--eqeqeq) Используйте `===` и `!==` вместо `==` и `!=`

  eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq)

<a name="comparison--if"></a><a name="15.2"></a>
- [15.2](#comparison--if) Условные конструкции, например, `if` вычисляют выражение используя приведение типов по следующим правилам:

  - **Объекты** вычисляются в **true**
  - **Undefined** вычисляется в **false**
  - **Null** вычисляется в **false**
  - **Логические типы** вычисляются напрямую без приведения типов
  - **Number** вычисляется в **false** если значение **+0**, **-0** или **NaN**, иначе в **true**
  - **Строки** вычисляются в **false** если значение - пустая строка (`''`), иначе в **true**

  ```javascript
  if ([0] && []) {
    // true
    // массив (даже пустой) - объект; объекты вычисляются в true
  }
  ```

<a name="comparison--shortcuts"></a><a name="15.3"></a>
- [15.3](#comparison--shortcuts) Используйте сокращенный синтаксис для переменных типа `boolean` и полный синтаксис для переменных типа `String` и `Number`

  ```javascript
  // плохо
  if (isValid === true) {
    // ...
  }

  // хорошо
  if (isValid) {
    // ...
  }

  // плохо
  if (name) {
    // ...
  }

  // хорошо
  if (name !== '') {
    // ...
  }

  // плохо
  if (collection.length) {
    // ...
  }

  // хорошо
  if (collection.length > 0) {
    // ...
  }
  ```

<a name="comparison--switch-blocks"></a><a name="15.4"></a>
- [15.4](#comparison--switch-blocks) Используйте фигурные скобки для задания блоков кода в `case` и `default` в случаях когда там есть объявления (`let`, `const`, `function` или `class`)

  eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations)

  >Почему: объявленные переменные видны во всем `switch`, но инициализируются они только в своем `case`; это может вызвать проблемы в случаях когда разные `case` объявляют одинаковые переменные

  ```javascript
  // плохо
  switch (foo) {
    case 1:
      let x = 1;
      break;
    case 2:
      const y = 2;
      break;
    case 3:
      function f () {
        // ...
      }
      break;
    default:
      class C {}
  }

  // хорошо
  switch (foo) {
    case 1: {
      let x = 1;
      break;
    }
    case 2: {
      const y = 2;
      break;
    }
    case 3: {
      function f () {
        // ...
      }
      break;
    }
    case 4:
      foo();
      break;
    default: {
      class C {}
    }
  }
  ```

<a name="comparison--nested-ternaries"></a><a name="15.5"></a>
- [15.5](#comparison--nested-ternaries) Тернарные выражения не должны вкладываться друг в друга и, в целом, должны быть однострочными

  eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary)

  ```javascript
  // плохо
  const foo = maybe1 > maybe2
    ? 'bar'
    : value1 > value2 ? 'baz' : null;

  const maybeNull = value1 > value2 ? 'baz' : null;

  // лучше
  const foo = maybe1 > maybe2
    ? 'bar'
    : maybeNull;

  // наилучший способ
  const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
  ```

<a name="comparison--unneeded-ternary"></a><a name="15.6"></a>
- [15.6](#comparison--unneeded-ternary) Избегайте ненужных тернарных выражений

  eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary)

  ```javascript
  // плохо
  const foo = a ? a : b;
  const bar = c ? true : false;
  const baz = c ? false : true;

  // хорошо
  const foo = a || b;
  const bar = !!c;
  const baz = !c;
  ```

<a name="comparison--no-mixed-operators"></a><a name="15.7"></a>
- [15.7](#comparison--no-mixed-operators) Оборачивайте операторы в скобки когда они смешаны в утверждении; при смешении арифметических операторов не смешивайте `**` и `%` с ними же или с `+`, `-`, `*` и `/`

  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators)

  >Почему: это улучшает читаемость и проясняет намерения разработчика

  ```javascript
  // плохо
  const foo = a && b < 0 || c > 0 || d + 1 === 0;

  // плохо
  const bar = a ** b - 5 % d;

  // плохо
  if (a || b && c) {
    return d;
  }

  // хорошо
  const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

  // хорошо
  const bar = (a ** b) - (5 % d);

  // хорошо
  if ((a || b) && c) {
    return d;
  }

  // хорошо
  const bar = a + b / c * d;
  ```

**[К содержанию](#table-of-contents)**

## Блоки <a name="blocks"></a>

<a name="blocks--braces"></a><a name="16.1"></a>
- [16.1](#blocks--braces) Используйте фигурные скобки с любыми многострочными блоками

  eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

  ```javascript
  // плохо
  if (test)
    return false;

  // хорошо
  if (test) {
    return false;
  }

  // плохо
  function foo () { return false; }

  // хорошо
  function bar () {
    return false;
  }
  ```

<a name="blocks--cuddled-elses"></a><a name="16.2"></a>
- [16.2](#blocks--cuddled-elses) Помещайте `else` на одну строку с закрывающей скобкой `if`

  eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style)

  ```javascript
  // плохо
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  }

  // хорошо
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```

<a name="blocks--no-else-return"></a><a name="16.3"></a>
- [16.3](#blocks--no-else-return) Если в блоке `if` всегда используется `return` последний блок `else` не нужен; `return` в блоке `esle if`, который следует за блоком `if`, использующим `return` может быть разделен на несколько блоков `if`

  eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

  ```javascript
  // плохо
  function foo () {
    if (x) {
      return x;
    } else {
      return y;
    }
  }

  // плохо
  function cats () {
    if (x) {
      return x;
    } else if (y) {
      return y;
    }
  }

  // плохо
  function dogs () {
    if (x) {
      return x;
    } else {
      if (y) {
        return y;
      }
    }
  }

  // хорошо
  function foo () {
    if (x) {
      return x;
    }

    return y;
  }

  // хорошо
  function cats () {
    if (x) {
      return x;
    }

    if (y) {
      return y;
    }
  }

  // хорошо
  function dogs (x) {
    if (x) {
      if (z) {
        return y;
      }
    } else {
      return z;
    }
  }
  ```

**[К содержанию](#table-of-contents)**