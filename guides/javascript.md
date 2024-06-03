# Руководство по стилю кода JavaScript

В основе этого документа лежит [руководство по стилю кода Javascript Airbnb](https://github.com/airbnb/javascript). Это руководство не является точным переводом и в нем есть отличия от оригинала.

Другие руководства:

- [по стилю кода React/Redux](react.md)
- [по стилю кода CSS/Less](css-less.md)
- [по самообучению](frontend-roadmap.md)

## Содержание <a name="table-of-contents"></a>

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
1. [Управление выполнением](#control-statements)
1. [Комментарии](#comments)
1. [Пробельные символы](#whitespace)
1. [Запятые](#commas)
1. [Точки с запятой](#semicolons)
1. [Явное и неявное приведение типов](#coercion)
1. [Именование](#naming)
1. [Методы доступа](#accessors)
1. [События](#events)
1. [jQuery](#jquery)
1. [Совместимость с ECMAScript 5](#es5-compat)
1. [Стиль написания ECMAScript 6+ (ES 2015+)](#es6-styles)
1. [Стандартная библиотека](#standard-library)
1. [TypeScript](#typescript)

## Типы <a name="types"></a>

<a name="types--primitives"></a><a name="1.1"></a>
- [1.1](#types--primitives) **Примитивы**: когда вы работаете с примитивом, вы работаете напрямую с его значением.

  - `string`,
  - `number`,
  - `boolean`,
  - `null`,
  - `undefined`,
  - `symbol`.

  ```javascript
  const foo = 1;
  let bar = foo;

  bar = 9;

  console.log(foo, bar); // 1, 9
  ```

<a name="types--complex"></a><a name="1.2"></a>
- [1.2](#types--complex) **Сложные типы**: когда вы работаете со сложным типом, вы работаете со ссылкой на его значение.

  - `object`,
  - `array`,
  - `function`.

  ```javascript
  const foo = [1, 2];
  const bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // 9, 9
  ```

**[К содержанию](#table-of-contents)**

## Ссылки <a name="references"></a>

<a name="references--prefer-const"></a><a name="2.1"></a>
- [2.1](#references--prefer-const) Для всех ссылок используйте `const`, избегайте использования `var`.

  eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign)

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
- [2.2](#references--disallow-var) Если необходимо переопределять ссылки, используйте `let` вместо `var`.

  eslint: [`no-var`](https://eslint.org/docs/rules/no-var)

  >Почему: у `let` уже область видимости &mdash; блок, вместо функции у `var`

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
- [2.3](#references--block-scope) Обратите внимание, что область видимости `let` и `const` &mdash; блок.

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
- [3.1](#objects--no-new) Для объявления объекта используйте литерал.

  eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object)

  >Почему: более краткий синтаксис

  ```javascript
  // плохо
  const item = new Object();

  // хорошо
  const item = {};
  ```

<a name="objects--es6-computed-properties"></a><a name="3.2"></a>
- [3.2](#objects--es6-computed-properties) При создании объектов с динамическими именами свойств используйте вычисляемые имена свойств.

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
- [3.3](#objects--es6-object-shorthand) Используйте сокращенный синтаксис методов.

  eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

  >Почему: более краткий синтаксис

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
- [3.4](#objects--es6-object-concise) Используйте сокращенный синтаксис свойств.

  eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

  >Почему: более краткий и описательный синтаксис

  ```javascript
  const name = 'John Doe';

  // плохо
  const user = {
    name: name
  };

  // хорошо
  const user = {
    name
  };
  ```

<a name="objects--alpha-order"></a><a name="3.5"></a>
- [3.5](#objects--alpha-order) Располагайте свойства в алфавитном порядке.

  >Почему: так легче найти нужное свойство в объектах с большим количеством свойств.

  ```javascript
  // плохо
  const user = {
    login,
    permissions,
    email,
    name,
    location,
    token
  };

  // хорошо
  const user = {
    email,
    location,
    login,
    name,
    permissions,
    token
  };
  ```

<a name="objects--quoted-props"></a><a name="3.6"></a>
- [3.6](#objects--quoted-props) Заключайте в кавычки только те свойства, имена которых не являются корректными идентификаторами.

  eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)

  >Почему: Субъективно легче читать; улучшает подсветку синтаксиса; улучшает оптимизацию разными JS-движками.

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
- [3.7](#objects--prototype-builtins) Не вызывайте напрямую методы `Object.prototype` (например, `hasOwnProperty`, `propertyIsEnumerable`, `isPrototypeOf`).

  >Почему: эти свойства могут быть переопределены, или объект может быть нулевым.

  ```javascript
  // плохо
  console.log(object.hasOwnProperty(key));

  // хорошо
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // наилучший способ
  // файл helpers.js
  export const has = Object.prototype.hasOwnProperty; // кеш

  // файл script.js
  import has from 'has';
  // ...
  console.log(has.call(object, key));
  ```

  ```javascript
  // плохо: при исполнении кода будет выброшена ошибка из-за того, что объект оказался нулевым (null)
  const nullObj = Object.create(null);
  console.log(nullObj.hasOwnProperty('a')); // Uncaught TypeError: nullObj.hasOwnProperty is not a function
  console.log(Object.prototype.hasOwnProperty.call(nullObj, 'a')); // false

  // плохо: при исполнении кода будет выброшена ошибка в случае, если метод подменен
  const obj = {a: 1, b: 2};
  Object.prototype.hasOwnProperty = false;
  console.log(obj.hasOwnProperty('a')); // Uncaught TypeError: nullObj.hasOwnProperty is not a function
  console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // Uncaught TypeError: Object.prototype.hasOwnProperty.call is not a function

  // хорошо: при исполнении кода ошибки отсутствуют, работа не зависит от того, является ли объект нулевым
  const has = Object.prototype.hasOwnProperty;
  Object.prototype.hasOwnProperty = false; // ссылка на метод сохранена в переменной has
  console.log(has.call(obj, 'a'));
  ```

<a name="objects--rest-spread"></a><a name="3.8"></a>
- [3.8](#objects--rest-spread) Используйте `spread`-оператор вместо [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) для поверхностного копирования объектов; используйте `rest`-оператор для создания объекта с оставшимися свойствами исходного.

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

## Массивы <a name="arrays"></a>

<a name="arrays--literals"></a><a name="4.1"></a>
- [4.1](#arrays--literals) Для объявления массива используйте литерал.

  eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor)

  >Почему: глобальное значение `Array` может быть переопрапределено; разный результат использования конструктора при использовании одного и нескольких аргументов.

  ```javascript
  // плохо
  const items = new Array();

  const ids = new Array(1, 2, 3);
  ids.toString(); // 1,2,3

  const numbers = new Array(3);
  numbers.push(4);
  numbers.toString(); // ,,,4

  // хорошо
  const items = [];
  const ids = [1, 2, 3];
  ```

<a name="arrays--push"></a><a name="4.2"></a>
- [4.2](#arrays--push) Используйте [Array#push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) вместо прямого присваивания значений элементам массива.

  ```javascript
  const someStack = [];

  // плохо
  someStack[someStack.length] = 'some value';

  // хорошо
  someStack.push('some value');
  ```

<a name="arrays--es6-array-spreads"></a><a name="4.3"></a>
- [4.3](#arrays--es6-array-spreads) Для копирования массивов используйте `spread`-оператор (`...`).

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
- [4.4](#arrays--from) Чтобы сконвертировать массивоподобный объект в массив используйте `spread`-оператор (`...`) вместо [Array#from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

  ```javascript
  const foo = document.querySelectorAll('.foo');

  // плохо
  const nodes = Array.from(foo);

  // хорошо
  const nodes = [...foo];
  ```

<a name="arrays--mapping"></a><a name="4.5"></a>
- [4.5](#arrays--mapping) Испорльзуйте [Array#from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) вместо `spread`-оператора (`...`) при маппинге по итерируемым типам.

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

  // плохо - отсутствие возвращаемого значения приведет к тому, что `memo` будет `undefined` после первой итерации
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
- [4.7](#arrays--bracket-newline) В многострочном массиве используйте переводы строк после открывающей скобки и перед закрывающей скобкой.

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
- [5.1](#destructuring--object) При необходимости использования нескольких свойств объекта используйте деструктурирующее присваивание.

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
- [5.2](#destructuring--array) Используйте деструктурирующее присваивание при работе с массивами.

  ```javascript
  const arr = [1, 2, 3];

  // плохо
  const first = arr[0];
  const second = arr[1];

  // хорошо
  const [first, second] = arr;
  ```

<a name="destructuring--object-over-array"></a><a name="5.3"></a>
- [5.3](#destructuring--object-over-array) Используйте деструктурирование объекта, если необходимо вернуть несколько значений.

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
- [6.1](#strings--quotes) Используйте одинарные кавычки (`''`) для строк.

  eslint: [`quotes`](https://eslint.org/docs/rules/quotes)

  ```javascript
  // плохо
  const name = "Obi-Wan Kenobi";

  // плохо
  const name = `Obi-Wan Kenobi`;

  // хорошо
  const name = 'Obi-Wan Kenobi';
  ```

<a name="strings--line-length"></a><a name="6.2"></a>
- [6.2](#strings--line-length) Не переносите длинный текст.

  >Почему: с перенесенными строками сложнее работать и текст сложнее найти используя функцию поиска `IDE`

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
- [6.3](#strings--es6-template-literals) При программной генерации строк используйте строковые шаблоны вместо конкатенации.

  eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template), [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

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
- [6.4](#strings--eval) Никогда не используейте `eval()` на строках, это открывает слишком много уязвимостей.

  eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

<a name="strings--escaping"><a name="6.5"></a>
- [6.5](#strings-escaping) Не используйте экранирование символов без необходимости.

  eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

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

<a name="functions--declarations"></a><a name="7.1"></a>
- [7.1](#function--declarations) Используйте функциональные выражения вместо объявлений функций.

  eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

  >Почему: объявления функций поднимаются, поэтому слишком легко сослаться на функцию до ее определения в файле. Это вредит читаемости и поддерживаемости. Если вам кажется, что объявление функции настолько большое или сложное, что это мешает понимать остальную часть файла, возможно, стоит выделить его в отдельный модуль. Не забывайте явно именовать выражение, не важно будет ли потом извлекаться из переменной имя (что часто происходит в современных браузерах или при использовании компиляторов типа `Babel`) - это облегчит поиск проблемы в стеке вызовов.

  ```javascript
  // плохо
  function foo () {
    // ...
  }

  // плохо
  const foo = function () {
    // ...
  }

  // хорошо
  const short = function longUniqueMoreDescriptiveLexicalFoo () {
    // ...
  }
  ```

<a name="functions--iife"></a><a name="7.2"></a>
- [7.2](#functions--iife) Оборачивайте скобками немедленно выполняемые функциональные выражения.

  eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife)

  >Почему: немедленно выполняемые функциональные выражения - отдельный модуль. Оборачивание в скобки самого выражения и скобок, указывающих на его вызов, четко указывает на это.

  ```javascript
  (function () {
    console.log('iife');
  }());
  ```

<a name="functions--in-blocks"></a><a name="7.3"></a>
- [7.3](#functions--in-blocks) Никогда не объявляйте функции в нефункциональном блоке (if, while и пр.). Вместо этого присваивайте объявление функции переменной. Хотя браузеры и позволяют делать это, они интерпретируют такой код по-разному.

  eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func)

<a name="functions--note-on-block"></a><a name="7.4"></a>
- [7.4](#functions--note-on-block) ECMA-262 определяет `блок` как набор утверждений. Объявление функции не является утверждением.

  ```javascript
  // плохо
  if (currentUser) {
    function test() {
      console.log('Плохо');
    }
  }

  // хорошо
  let test;
  if (currentUser) {
    test = () => {
      console.log('Хорошо');
    };
  }
  ```

<a name="functions--arguments-shadow"></a><a name="7.5"></a>
- [7.5](#functions--arguments-shadow) Никогда не называйте параметр `arguments`. Он перекроет объект `arguments`.

  ```javascript
  // плохо
  function foo (name, options, arguments) {
    // ...
  }

  // хорошо
  function foo (name, options, args) {
    // ...
  }
  ```

<a name="functions--es6-rest"></a><a name="7.6"></a>
- [7.6](#functions--es6-rest) Никогда не используйте `arguments`, используйте `rest`-синтаксис `...`.

  eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

  >Почему: `...` явно указывает на то, какие аргументы используются. Кроме того `...` вернет настоящий массив, а не массивоподобный объект типа `arguments`.

  ```javascript
  // плохо
  function concatenateAll() {
    const args = Array.prototype.slice.call(arguments);
    return args.join('');
  }

  // хорошо
  function concatenateAll(...args) {
    return args.join('');
  }
  ```

<a name="functions--es6-default-parameters"></a><a name="7.7"></a>
- [7.7](#functions--es6-default-parameters) Используйте синтаксис значений аргументов по умолчанию вместо того, чтобы изменять аргументы.

  ```javascript
  // очень плохо
  function handleThings (opts) {
    // Не стоит изменять агрументы функции
    // Дополнительно: если opts - false, то после такого присваивания opts станет true
    opts = opts || {};
    // ...
  }

  // все еще плохо
  function handleThings (opts) {
    if (opts === void 0) {
      opts = {};
    }
    // ...
  }

  // хорошо
  function handleThings (opts = {}) {
    // ...
  }
  ```

<a name="functions--default-side-effects"></a><a name="7.8"></a>
- [7.8](#functions--default-side-effects) Избегайте побочных эффектов в значениях параметров по умолчанию.

  ```javascript
  var b = 1;
  // плохо
  function count (a = b++) {
    console.log(a);
  }

  count(); // 1
  count(); // 2
  count(3); // 3
  count(); // 3
  ```

<a name="functions--defaults-last"></a><a name="7.9"></a>
- [7.9](#functions--defaults-last) Всегда помещайте необязательные параметры в конец.

  ```javascript
  // плохо
  function handleThings (opts = {}, name) {
    // ...
  }

  // хорошо
  function handleThings (name, opts = {}) {
    // ...
  }
  ```

<a name="functions--constructor"></a><a name="7.10"></a>
- [7.10](#functions--constructor) Никогда не используйте конструктор `Function` для создания новой функции.

  eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

  >Почему: при создании функции таким способом используется механизм наподобие `eval()`, который открывает уязвимости

  ```javascript
  // плохо
  var add = new Function('a', 'b', 'return a + b');

  // все еще плохо
  var subtract = Function('a', 'b', 'return a - b');
  ```

<a name="functions--signature-spacing"></a><a name="7.11"></a>
- [7.11](#functions--signature-spacing) Пробельные символы в сигнатуре функции

  eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren), [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks), [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing)

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

<a name="functions--mutate-params"></a><a name="7.12"></a>
- [7.12](#functions--mutate-params) Никогда не изменяйте параметры.

  eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign)

  >Почему: изменение параметров может иметь нежелательные побочные эффекты в вызывающем коде

  ```javascript
  // плохо
  function f1 (obj) {
    obj.key = 1;
  }

  // хорошо
  function f2 (obj) {
    const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
  }
  ```

<a name="functions--reassign-params"></a><a name="7.13"></a>
- [7.13](#functions--reassign-params) Никогда не переопределяйте параметры.

  eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign)

  >Почему: переопределение параметров может привести к неожиданному поведению, особенно во время доступа к `arguments`. Это также может оказывать воздействие на оптимизацию, особенно в `V8`.

  ```javascript
  // плохо
  function f1 (a) {
    a = 1;
    // ...
  }

  function f2 (a) {
    if (!a) {
      a = 1;
    }
    // ...
  }

  // хорошо
  function f3 (a) {
    const b = a || 1;
    // ...
  }

  function f4 (a = 1) {
    // ...
  }
  ```

<a name="functions--spread-vs-apply"></a><a name="7.14"></a>
- [7.14](#functions--spread-vs-apply) Предпочитайте использовать `spread`-оператор (`...`) при вызове вариативных функций.

  eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

  >Почему: такой синтаксис проще и чище, не надо передавать контекст и сложнее совместно использовать `new` и `apply`

  ```javascript
  // плохо
  const x = [1, 2, 3, 4, 5];
  console.log.apply(console, x);

  // хорошо
  const x = [1, 2, 3, 4, 5];
  console.log(...x);

  // плохо
  new (Function.prototype.bind.apply(Date, [null, 2018, 8, 5]));

  // хорошо
  new Date(...[2018, 8, 5]);
  ```

<a name="functions--signature-invocation-indentation"></a><a name="7.15"></a>
- [7.15](#functions--signature-invocation-indentation) Функции с многострочными сигнатурами или вызовами, должны подчиняться тем же правилам форматирования, что и прочие списки: каждый элемент списка на своей строке с висящей запятой у последнего элемента.

  >Почему: более простые `diff`-ы

  ```javascript
  // плохо
  function foo (bar,
                baz,
                quux) {
    // ...
  }

  // хорошо
  function foo (
    bar,
    baz,
    quux,
  ) {
    // ...
  }

  // плохо
  console.log(foo,
    bar,
    baz);

  // хорошо
  console.log(
    foo,
    bar,
    baz,
  );
  ```

**[К содержанию](#table-of-contents)**

## Стрелочные функции <a name="arrow-functions"></a>

<a name="arrow-functions--use-them"></a><a name="8.1"></a>
- [8.1](#arrow-functions--use-them) При необходимости использовать функционально выражение (например, при передаче в качестве аргумента безымянной функции) используйте стрелочные функции.

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
- [8.2](#arrow-functions--implicit-return) Если тело функции состоит из одной строки, которая возвращает [выражение](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) без побочных эффектов, опустите фигурные скобки и используйте неявный `return`.

  eslint: [`arrow-parens`](https://esling.org/docs/rules/arrow-parens), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style)

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
- [8.3](#arrow-functions--paren-wrap) В случае, когда выражение занимает несколько строк, оберните его в скобки для лучшей читаемости.

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
- [8.5](#arrow-functions--confusing) Избегайте использования синтаксиса стрелочных функций (`=>`) с синтаксисом оперторов сравнения (`<=`, `=>`).

  eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

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
- [9.1](#constructors--use-class) Всегда используйте `class`, избегайте прямого изменения `prototype`.

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
- [9.2](#constructors--extends) Используйте `extends` для наследования.

  >Почему: это встроенный способ наследования финкциональности прототипа без необходимости ломать `instanceof`.

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
- [9.3](#constructors--chaining) Методы могут возвращать `this` для обеспечения чейнинга.

  ```javascript
  // плохо
  Mage.prototype.fly = function () {
    this.flying = true;
    return true;
  };

  Mage.prototype.setHeight = function (height) {
    this.height = height;
  };

  const mage = new Mage();

  mage.fly(); // => true
  mage.setHeight(20); // undefined

  // хорошо
  class Mage {
    fly () {
      this.flying = true;
      return this;
    }
  
    setHeight (height) {
      this.height = height;
      return this;
    }
  }

  const mage = new Mage();

  mage.fly()
    .setHeight(20);
  ```

<a name="constructors--tostring"></a><a name="9.4"></a>
- [9.4](#constructors--tostring) Ничего плохого в написании своих методов `toString`, если вы убедились, что они работают нормально и не имеют побочных эффектов.

  ```javascript
  class Person {
    constructor (options = {}) {
      this.name = options.name || 'no name';
    }

    getName () {
      return this.name;
    }

    toString () {
      return `Person - ${this.getName()}`;
    }
  }
  ```

<a name="constructors--no-useless"></a><a name="9.5"></a>
- [9.5](#constructors--no-useless) У классов есть конструктор по-умолчанию, если не указан собственный конструктор; пустой конструктор или конструктор, только вызывающий родительский конструктор бесполезен.

  eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

  ```javascript
  // плохо
  class Animal {
    constructor () {}

    getName () {
      return this.name;
    }
  }

  // плохо
  class Rabbit extends Animal {
    constructor (...args) {
      super(...args);
    }
  }

  // хорошо
  class Rabbit extends Animal {
    constructor (...args) {
      super(...args);
      this.name = 'Bugs';
    }
  }
  ```

<a name="classes--no-duplicate-members"></a><a name="9.6"></a>
- [9.6](#classes--no-duplicate-members) Избегайте повторов членов класса.

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

  // хорошо
  import {AwesomeFeature} from './SuperModule';
  export {
    AwesomeFeature
  };

  // отлично
  export {AwesomeFeature} from './SuperModule';
  ```

<a name="modules--no-wildcard"></a><a name="10.2"></a>
- [10.2](#modules--no-wildcard) Не используйте «звездочку» при импорте.

  >Почему: это дает уверенность, что есть только один экспорт по умолчанию

  ```javascript
  // плохо
  import * as SuperModule from './SuperModule';

  // хорошо
  import SuperModule from './SuperModule';
  ```

<a name="modules--no-default-export"></a><a name="10.3"></a>
- [10.3](#modules--no-default-export) Не используйте экспорт по умолчанию.

  >Почему: экспорт/импорт по умолчанию усложняет рефакторинг и использование `IDE`; отсутствие импортов по умолчанию сделает блок импортов более однородным

  ```javascript
  // плохо
  // файл AwesomeFeature.js
  export {AwesomeFeature as default} from './SuperModule';

  // плохо
  // файл AwesomeFeature.js
  import {AwesomeFeature} from './SuperModule';
  export default AwesomeFeature;

  // хорошо
  // файл AwesomeFeature.js
  import {AwesomeFeature} from './SuperModule';
  export {
    AwesomeFeature
  };

  // отлично
  // файл AwesomeFeature.js
  export {AwesomeFeature} from './SuperModule';
  ```

<a name="modules--no-duplicate-imports"></a><a name="10.4"></a>
- [10.4](#modules--no-duplicate-imports) Единственный импорт для каждого источника.

  eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

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
- [10.5](#modules--no-mutable-exports) Экспортируйте только константы.

  eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

  >Почему: в целом, стоит избегать изменений значений (mutations). В частности - стоит экспортировать только константы. Это обеспечивает предсказуемость результата.

  ```javascript
  // плохо
  let foo = 'bar';
  export {
    foo
  };

  // хорошо
  const foo = 'bar';
  export {
    foo
  };
  ```

<a name="modules--prefer-declaration-export"></a><a name="10.6"></a>
- [10.6](#modules--prefer-declaration-export) Даже если в модуле только один экспорт, предпочитайте экспорт объявления экспорту по умолчанию.

  eslint: [`import/no-default-export`](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-default-export.md)

  >Почему: экспорт/импорт по умолчанию затрудняет рефакторинг и использование `IDE`; отсутствие импортов по умолчанию сделает блок импортов более однородным

  ```javascript
  // плохо
  export default function foo () {};

  // хорошо
  export function foo () {};
  ```

<a name="modules--imports-first"></a><a name="10.7"></a>
- [10.7](#modules--imports-first) Помещайте строки импорта выше строк кода.

  eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

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
- [10.8](#modules--multiline-imports-over-newlines) Многострочные импорты должны форматироваться так же как многострочные массивы или литералы объектов.

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
    longName9,
  } from 'module';
  ```

<a name="modules--no-webpack-loader-syntax"></a><a name="10.9"></a>
- [10.9](#modules--no-webpack-loader-syntax) Не указывайте загрузчики `Webpack`-а в импортах.

  eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

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
- [11.1](#iterators--nope) Не используйте итераторы, предпочитайте функции высшего порядка циклам типа `for-in` и `for-of`.

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
- [11.2](#generators--nope) Не используйте генераторы пока.

  >Почему: они не очень хорошо переводятся в ES5

<a name="generators--spacing"></a><a name="11.3"></a>
- [11.3](#generators-spacing) Если использование генераторов - необходимость, используйте пробельные символы в сигнатуре функции правильно.

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
- [12.1](#properties--dot) Используйте «точку» при получении доступа к свойству.

  eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation)

  ```javascript
  const luke = {
    jedi: true
  };

  // плохо
  const isJedi = kogoruhn['jedi'];

  // хорошо
  const isJedi = kogoruhn.jedi;
  ```

<a name="properties--bracket"></a><a name="12.2"></a>
- [12.2](#properties--bracket) Используйте квадратные скобки (`[]`) при получении доступа к свойству с использованием переменной.

  ```javascript
  const luke = {
    jedi: true
  };

  function getProp (prop) {
    return luke[prop];
  }

  const isJedi = getProp('jedi');
  ```

<a name="properties--es2016-exponentiation-operator"></a><a name="12.3"></a>
- [12.3](#properties--es2016-expoenentiation-operator) Используйте оператор возведения в степень `**` при подсчете степени.

  eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties)

  ```javascript
  // плохо
  const binary = Math.pow(2, 10);

  // хорошо
  const binary = 2 ** 10;
  ```

**[К содержанию](#table-of-contents)**

## Переменные <a name="variables"></a>

<a name="variables--const"></a><a name="1.1"></a>
- [13.1](#variables--const) При объявлении переменных всегда используйте `const` или `let`.

  eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef), [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

  >Почему: использование `var` приводит к глобальным переменным и замусориванию глобальной области видимости

  ```javascript
  // плохо
  superPower = new SuperPower();

  // хорошо
  const superPower = new SuperPower();
  ```

<a name="variables--one-const"></a><a name="13.2"></a>
- [13.2](#variables--one-const) Используйте `const` или `let` для объявления одной переменной.

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
- [13.3](#variables--const-let-group) Группируйте все объявления `const`, потом группируйте все объявления `let`.

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
- [13.4](#variables--define-where-used) Присваивайте значения переменным там, где вы их используете, но помещайте их в разумное место.

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
- [13.5](#variables--no-chain-assignment) Не присваивайте значения по цепочке.

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
- [13.6](#variables--unary-increment-decrement) Избегайте использования унарных операторов увеличения и уменьшения (++, --).

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
- [14.1](#hoisting--about) Объявления `var` поднимаются к началу области видимости, присвоения им - нет; объявления `const` и `let` обладают временной мертвой зоной ([Temporal Dead Zone](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let)); также важно знать почему [typeof больше не безопасен](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

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
- [14.2](#hoisting--anon-expressions) Безымянные функциональные выражения поднимают название переменной, но не присвоение функции.

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
- [14.3](#hoisting--named-expressions) Именованные функциональные выражения поднимают название переменной, но не название и тело функции.

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
- [14.4](#hoisting--declarations) Объявления функций поднимают как имя, так и тело функции.

  ```javascript
  function example () {
    superPower(); // Flying

    function superPower () {
      console.log('Flying');
    }
  }
  ```

## Операторы сравнения и равенство <a name="comparison"></a>

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
- [15.3](#comparison--shortcuts) Используйте сокращенный синтаксис для переменных типа `Boolean`, `String` и `Number`.

  ```javascript
  // плохо
  if (isValid === true) {
    // ...
  }

  // плохо
  if (name !== '') {
    // ...
  }

  // плохо
  if (collection.length > 0) {
    // ...
  }

  // хорошо
  if (isValid) {
    // ...
  }

  // хорошо
  if (name) {
    // ...
  }

  // хорошо
  if (!!name) {
    // ...
  }

  // хорошо
  if (collection.length) {
    // ...
  }
  ```

<a name="comparison--switch-blocks"></a><a name="15.4"></a>
- [15.4](#comparison--switch-blocks) Используйте фигурные скобки для задания блоков кода в `case` и `default` в случаях, когда там есть объявления (`let`, `const`, `function` или `class`).

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
- [15.5](#comparison--nested-ternaries) Тернарные выражения не должны вкладываться друг в друга и, в целом, должны быть однострочными.

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
- [15.6](#comparison--unneeded-ternary) Избегайте ненужных тернарных выражений.

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
- [15.7](#comparison--no-mixed-operators) Оборачивайте операторы в скобки когда они смешаны в утверждении; при смешении арифметических операторов не смешивайте `**` и `%` с ними же или с `+`, `-`, `*` и `/`.

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
- [16.1](#blocks--braces) Используйте фигурные скобки с любыми многострочными блоками.

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
- [16.2](#blocks--cuddled-elses) Помещайте `else` на одну строку с закрывающей скобкой `if`.

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
- [16.3](#blocks--no-else-return) Если в блоке `if` всегда используется `return` последний блок `else` не нужен; `return` в блоке `esle if`, который следует за блоком `if`, использующим `return` может быть разделен на несколько блоков `if`.

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

## Управление выполнением <a name="control-statements"></a>

<a name="control-statements--formatting"></a><a name="17.1"></a>
- [17.1](#control-statements--formatting) Если управляющая конструкция (`if`, `while` и пр.) становится слишком длинной и превышает допустимый размер строки, каждое условие (или группа) должно начинаться с новой строки; условные операторы записываются в начале строки.

  >Почему: условные операторы выровнены (улучшает читаемость) и стиль написания сильно похож на чейнинг; легче «отключить» условие при помощи комментария

  ```javascript
  // плохо
  if ((foo == 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
    // ...
  }

  // плохо
  if (foo === 123 &&
      bar === 'abc') {
    // ...
  }

  // плохо
  if (foo === 123
      && bar === 'abc') {
    // ...
  }

  // плохо
  if (
    foo === 123 &&
    bar === 'abc'
  ) {
    // ...
  }

  // хорошо
  if (
    foo === 123
    && bar === 'abc'
  ) {
    // ...
  }

  // хорошо
  if (
    (foo === 123 && bar === 'abc')
    && doesItLookGoodWhenItBecomesThatLong()
    && isThisReallyHappening()
  ) {
    // ...
  }

  // хорошо
  if (foo === 123 && bar === 'abc') {
    // ...
  }
  ```

**[К содержанию](#table-of-contents)**

## Комментарии <a name="comments"></a>

<a name="comments--multiline"></a><a name="18.1"></a>
- [18.1](#comments--multiline) Для многострочных комментариев используйте `/** ... */`

  ```javascript
  // плохо
  // make() возвращает новый элемент
  // соответствующий тегу, переданному в качестве аргумента
  //
  // @param {String} tag
  // @return {Element} element
  function make (tag) {
    // ...
    return element;
  }

  // хорошо
  /**
   * make() возвращает новый элемент
   * соответствующий тегу, переданному в качестве аргумента
   *
   * @param {String} tag
   * @return {Element} element
   */
  function make (tag) {
    // ...
    return element;
  }
  ```

<a name="comments--singleline"></a><a name="18.2"></a>
- [18.2](#comments--singleline) Для однострочных комментариев используйте `//`. Располагайте комментарий в отдельной строке над комментируемой. Вставляйте пустую строку перед комментарием, если только комментарий не первая строка блока.

  ```javascript
  // плохо
  const active = true; // is current tab

  // хорошо
  // is current tab
  const active = true;

  // плохо
  function getType () {
    console.log('fetching type...');
    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }

  // хорошо
  function getType () {
    console.log('fetching type...');

    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }

  // хорошо
  function getType () {
    // set the default type to 'no type';
    const type = this.type || 'no type';

    return type;
  }
  ```

<a name="comments--spaces"></a><a name="18.3"></a>
- [18.3](#comments--spaces) Начинайте все комментарии с пробела - так их легче читать.

  eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

  ```javascript
  // плохо
  //is current tab
  const active = true;

  // хорошо
  // is current tab
  const active = true;

  // плохо
  /**
   *make() возвращает новый элемент
   *соответствующий тегу, переданному в качестве аргумента
   *
   *@param {String} tag
   *@return {Element} element
   */
  function make (tag) {
    // ...
    return element;
  }

  // хорошо
  /**
   * make() возвращает новый элемент
   * соответствующий тегу, переданному в качестве аргумента
   *
   * @param {String} tag
   * @return {Element} element
   */
  function make (tag) {
    // ...
    return element;
  }
  ```

<a name="comments--actionitems"></a><a name="18.4"></a>
- [18.4](#comments--actionitems) Использование префиксов `FIXME` и `TODO` в комментариях помогает другим разработчикам быстро понять указываете ли вы на проблему, которую надо рассмотреть, или предлагаете решение проблемы, которое надо реализовать.

<a name="comments--fixme"></a><a name="18.5"></a>
- [18.5](#comments--fixme) Используйте `FIXME` для описания проблемы.

  ```javascript
  class Calculator extends Abacus {
    constructor () {
      super();

      // FIXME: не стоит использовать глобальные переменные здесь
      total = 0;
    }
  }
  ```

<a name="comments--todo"></a><a name="18.6"></a>
- [18.6](#comments--todo) Используйте `TODO` для описания решений проблем.

  ```javascript
  class Calculator extends Abacus {
    constructor () {
      super();

      // TODO: начальное значение total должно задаваться входными параметрами
      this.total = 0;
    }
  }
  ```


<a name="comments--todo"></a><a name="18.7"></a>
- [18.7](#comments--todo) Используйте [jsDoc](https://jsdoc.app) для документирования функций. Все утилитарные функции должны быть документированы.

  ```javascript
  // плохо
  const calcTotal = (objects, attrName) => {
    return objects.reduce((total, item) => total+= item[attrName], 0);
  }
  
  // хорошо
  /**
   * Возвращает сумму значений в атрибутах `attrName` из списка `objects`.
   * @param {Object[]} list - список объектов произвольного типа, которые содержат атрибут `attrName`.
   * @param {string} attrName - код атрибута объекта для подсчета суммы.
   * @returns {number} - сумма значений в атрибутах `attrName` из списка `objects`.
   */
  const calcTotal = (list, attrName) => {
    return list.reduce((total, item) => total += item[attrName], 0);
  }
  ```

**[К содержанию](#table-of-contents)**

## Пробельные символы <a name="whitespace"></a>

<a name="whitespaces--tabs-over-spaces"></a><a name="19.1"></a>
- [19.1](#whitespaces--tabs-over-spaces) Используйте табуляцию для отступов и пробелы для выравнивания.

  >Почему: ширина табов может быть разной, но кодовая база при этом неизменна.

  eslint: [`indent`](https://eslint.org/docs/rules/indent)

  ```javascript
  // плохо
  function foo () {
  ····let name;
  }

  // плохо
  function foo () {
  ·let name;
  }

  // хорошо
  function foo () {
  ⇥let name;
  }
  ```

<a name="whitespace--before-blocks"></a><a name="19.2"></a>
- [19.2](#whitespace--before-blocks) Помещайте один пробел перед открывающей скобкой.

  eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

  ```javascript
  // плохо
  function test (){
    console.log('test');
  }

  // хорошо
  function test () {
    console.log('test');
  }

  // плохо
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog'
  });

  // хорошо
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog'
  });
  ```

<a name="whitespace--around-keywords"></a><a name="19.3"></a>
- [19.3](#whitespace--around-keywords) Помещайте один пробел перед открывающей скобкой в управляющих выражениях (`if`, `while` и пр.). При вызове функции между именем функции и списком аргументов не должно быть пробелов.

  eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing)

  ```javascript
  // плохо
  if(isOk()) {
    doSomething ();
  }

  // хорошо
  if (isOk()) {
    doSomething();
  }
  ```

<a name="whitespace--infix-ops"></a><a name="19.4"></a>
- [19.4](#whitespace--infix-ops) Обрамляйте операторы пробелами.

  eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops)

  ```javascript
  // плохо
  const x=y+5;

  // хорошо
  const x = y + 5;
  ```

<a name="whitespace--newline-at-end"></a><a name="19.5"></a>
- [19.5](#whitespace--newline-at-end) В конце файла должен быть символ перевода строки.

  eslint: [`eol-last`](https://eslint.org/docs/rules/eol-last)

  ```javascript
  // плохо
  import SuperComponent from './SuperComponent';
  // ...
  export {
    SuperComponent
  };
  ```

  ```javascript
  // плохо
  import SuperComponent from './SuperComponent';
  // ...
  export {
    SuperComponent
  };⏎
  ⏎
  ```

  ```javascript
  // хорошо
  import SuperComponent from './SuperComponent';
  // ...
  export {
    SuperComponent
  };⏎
  ```

<a name="whitespace--chains"></a><a name="19.6"></a>
- [19.6](#whitespace--chains) Используйте отступы в длинных цепочках вызовов (более 2 методов). Начинайте с точки, это подчеркивает, что в строке - вызов метода, а не новое утверждение.

  eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call)

  ```javascript
  // плохо
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // плохо
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();

  // хорошо
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // плохо
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', `translate(${radius + margin}, ${radius + margin})`)
    .call(tron.led);

  // хорошо
  const leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', `translate(${radius + margin}, ${radius + margin})`)
      .call(tron.led);

  // хорошо
  const leds = stage.selectAll('.led').data(data);
  ```

<a name="whitespace--after-blocks"></a><a name="19.7"></a>
- [19.7](#whitespace--after-blocks) Между блоком и следующим утверждением оставляйте пустую строку. Если тело блока состоит из блока констант и одной строки кода, необходимости в пустой строке нет.

  ```javascript
  // плохо
  if (foo) {
    return bar;
  }
  return baz;

  // хорошо
  if (foo) {
    return bar;
  }

  return baz;

  // плохо
  const obj = {
    foo () {
    },
    bar () {
    }
  };
  return obj;

  // хорошо
  const obj = {
    foo () {
    },

    bar () {
    }
  };

  return obj;

  // плохо
  const arr = [
    function foo () {
    },
    function bar () {
    }
  ];

  return arr;

  // хорошо
  const arr = [
    function foo () {
    },

    function bar () {
    }
  ];
  return arr;
  ```

<a name="whitespace--padded-blocks"></a><a name="19.8"></a>
- [19.8](#whitespace--padded-blocks) Не отбивайте блоки пустыми строками. Добавляйте пустые строки между логическими блоками.

  eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks)

  ```javascript
  // плохо
  function bar () {

    console.log(foo);

  }

  // плохо
  if (baz) {

    console.log(qux);
  } else {
    console.log(foo);

  }

  // плохо
  class Foo {

    constructor (bar) {
      this.bar = bar;
    }
  }

  // плохо
  class Bar {
    // ...

    method (a) {
      let b;
      if (a === 1) {
        b = c();
      }
      if (a === '1') {
        b = d();
      }
      return b;
    }
  }

  // хорошо
  function bar () {
    console.log(foo);
  }

  // хорошо
  if (baz) {
    console.log(qux);
  } else {
    console.log(foo);
  }

  // хорошо
  class Bar {
    // ...

    method (a) {
      let b;

      if (a === 1) {
        b = c();
      }

      if (a === '1') {
        b = d();
      }

      return b;
    }
  }
  ```

<a name="whitespace--in-parens"></a><a name="19.9"></a>
- [19.9](#whitespace--in-parens) Не добавляйте пробелы внутри круглых скобок.

  eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens)

  ```javascript
  // плохо
  function bar ( foo ) {
      // ...
  }

  // хорошо
  function bar (foo) {
    // ...
  }

  // плохо
  if ( foo ) {
    console.log(foo);
  }

  // хорошо
  if (foo) {
    console.log(foo);
  }
  ```

<a name="whitespace--in-brackets"></a><a name="19.10"></a>
- [19.10](#whitespace--in-brackets) Не добавляйте пробелы внутри квадратных скобок.

  eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing)

  ```javascript
  // плохо
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // хорошо
  const foo = [1, 2, 3];
  console.log(foo[0]);
  ```

<a name="whitespace--in-braces"></a><a name="19.11"></a>
- [19.11](#whitespace--in-braces) Не добавляйте пробелы внутри фигурных скобок.

  eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing)

  ```javascript
  // плохо
  const foo = { clark: 'kent' };

  // хорошо
  const foo = {clark: 'kent'};
  ```

<a name="whitespace--max-len"></a><a name="19.12"></a>
- [19.12](#whitespace--max-len) Избегайте строк, длина которых превышает 100 символов (включая пробелы). [Исключение](#strings--line-length) - строки текста, которые не надо переносить

  eslint: [`max-len`](https://eslint.org/docs/rules/max-len)

  >Почему: так удобнее читать и поддерживать код

  ```javascript
  // плохо
  const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

  // плохо
  $.ajax({ method: 'POST', url: 'http://example.com', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this.city.'));

  // хорошо
  const foo = jsonData
    && jsonData.foo
    && jsonData.foo.bar
    && jsonData.foo.bar.baz
    && jsonData.foo.bar.baz.quux
    && jsonData.foo.bar.baz.quux.xyzzy;

  // хорошо
  $.ajax({
    method: 'POST',
    url: 'http://example.com',
    data: { name: 'John' }
  })
    .done(() => console.log('Congratulations!'))
    .fail(() => console.log('You have failed this.city.'));
  ```


<a name="whitespace--max-len"></a><a name="19.12"></a>
- [19.12](#whitespace--empty-lines-in-logic-blocks) Группируйте логические блоки, отбивая их друг от друга пустой строкой

  >Почему: так удобнее читать и поддерживать код

  ```javascript
  // ужасно
  const successTagDivElement = document.createElement('div');
  const errorTagDivElement = document.createElement('div');
  successTagDivElement.innerText = 'Success';
  successTagDivElement.onClick = handleSuccessClick;
  errorTagDivElement.innerText = 'Error';
  errorTagDivElement.onClick = handleErrorClick;
  tags.append(successTagDivElement);
  tags.append(errorTagDivElement);
  ```

  // плохо
  const successTagDivElement = document.createElement('div');
  const errorTagDivElement = document.createElement('div');

  successTagDivElement.innerText = 'Success';
  successTagDivElement.onClick = handleSuccessClick;
  errorTagDivElement.innerText = 'Error';
  errorTagDivElement.onClick = handleErrorClick;
  tags.append(successTagDivElement);
  tags.append(errorTagDivElement);
  ```

  // хорошо
  const successTagDivElement = document.createElement('div');
  successTagDivElement.innerText = 'Success';
  successTagDivElement.onClick = handleSuccessClick;

  const errorTagDivElement = document.createElement('div');
  errorTagDivElement.innerText = 'Error';
  errorTagDivElement.onClick = handleErrorClick;
  
  tags.append(successTagDivElement);
  tags.append(errorTagDivElement);
  ```

  // хорошо
  const successTagDivElement = document.createElement('div');
  const errorTagDivElement = document.createElement('div');

  successTagDivElement.innerText = 'Success';
  successTagDivElement.onClick = handleSuccessClick;
  tags.append(successTagDivElement);

  errorTagDivElement.innerText = 'Error';
  errorTagDivElement.onClick = handleErrorClick;
  tags.append(errorTagDivElement);
  ```
**[К содержанию](#table-of-contents)**

## Запятые <a name="commas"></a>

<a name="commas--leading-trailing"></a><a name="20.1"></a>
- [20.1](#commas--leading-trailing) Не используйте запятые в начале строки.

  eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style)

  ```javascript
  // плохо
  const story = [
      once
    , upon
    , aTime
  ];

  // хорошо
  const story = [
    once,
    upon,
    aTime,
  ];

  // плохо
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };

  // хорошо
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

<a name="commas-dangling"></a><a name="20.2"></a>
- [20.2](#commas-dangling) Не используйте висячие запятые.

  eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle)

  >Почему: выглядит грамматически неправильно и отвлекает. Хотя при использовании висячих запятых `diff`-ы более чистые, на практике редки случаи, когда `diff`-ы без висячих запятых создавали сложности.

  ```javascript
  // плохо
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];

  // хорошо
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // плохо
  function createHero (
    firstName,
    lastName,
    inventorOf,
  ) {
    // ...
  }

  // хорошо
  function createHero (
    firstName,
    lastName,
    inventorOf
  ) {
    // ...
  }

  // хорошо (примечание: не должно быть запятой после '...rest')
  function createHero (
    firstName,
    lastName,
    inventorOf,
    ...heroArgs
  ) {
    // ...
  }

  // плохо
  createHero(
    firstName,
    lastName,
    inventorOf,
  );

  // хорошо
  createHero(
    firstName,
    lastName,
    inventorOf
  );

  // хорошо (примечание: не должно быть запятой после '...rest')
  createHero(
    firstName,
    lastName,
    inventorOf,
    ...heroArgs
  );
  ```

**[К содержанию](#table-of-contents)**

## Точки с запятой <a name="semicolons"></a>

<a name="semicolons--required"></a><a name="21.1"></a>
- [21.1](#semicolons-required) **Используйте**

  eslint: [`semi`](https://eslint.org/docs/rules/semi)

  ```javascript
  // плохо
  (function () {
    const name = 'John Connor'
    return name
  })()

  // хорошо
  (function () {
    const name = 'John Connor';
    return name;
  })();

  // хорошо, но legacy (не допускает использования функции как аргумента при соединении двух файлов с IIFE)
  ;((() => {
    const name = 'John Connor';
    return name;
  })());
  ```

  [Дополнительно про точки с запятой и IIFE](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214)

**[К содержанию](#table-of-contents)**

## Явное и неявное приведение типов <a name="coercion"></a>

<a name="coercion--explicit"></a><a name="22.1"></a>
- [22.1](#coercion--explicit) Явное приведение типов осуществляйте в начале утверждения.

<a name="coercion--strings"></a><a name="22.2"></a>
- [22.2](#coercion--strings) Для приведения к строке используйте `String`.

  eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```javascript
  // => this.reviewScore = 9;

  // плохо
  const totalScore = new String(this.reviewScore); // typeof totalScore 'object', а не 'string'

  // плохо
  const totalScore = this.reviewScore + ''; // вызывает this.reviewScore.valueOf()

  // плохо
  const totalScore = this.reviewScore.toString(); // не гарантирует, что будет возвращена строка

  // хорошо
  const totalScore = String(this.reviewScore);
  ```

<a name="coercion--numbers"></a><a name="22.3"></a>
- [22.3](#coercion--numbers) Используйте `Number` для явного приведения типов и `parseInt` (всегда с указанием основания) для преобразования строк.

  eslint: [`radix`](https://eslint.org/docs/rules/radix), [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```javascript
  const inputValue = '4';

  // плохо
  const val = new Number(inputValue);

  // плохо
  const val = +inputValue;

  // плохо
  const val = inputValue >> 0;

  // плохо
  const val = parseInt(inputValue);

  // хорошо
  const val = Number(inputValue);

  // хорошо
  const val = parseInt(inputValue, 10);
  ```

<a name="coercion--comment-deviations"></a><a name="22.4"></a>
- [22.4](#coercion--comment-deviations) Если по [причинам производительности](https://jsperf.com/coercion-vs-casting/3) необходимо использовать побитовый сдвиг вместо `parseInt` оставьте комментарий, что и зачем вы делаете

  ```javascript
  // хорошо
  /**
   * из-за parseInt код выполнялся медленно
   * побитовый сдвиг для преобразования строки в
   * число работает намного быстрее
   */
  const val = inputValue >> 0;
  ```

<a name="coercion--bitwise"></a><a name="22.5"></a>
- [22.5](#coercion--bitwise) При использовании побитового сдвига будьте осторожны. Числа представлены [64-битными значениями](https://es5.github.io/#x4.3.19) в то время как операции побитового сдвига всегда возвращают [32-битное целое](https://es5.github.io/#x11.7). Наибольшее 32-битное целое: 2 147 483 647.

  ```javascript
  2147483647 >> 0; // => 2147483647
  2147483648 >> 0; // => -2147483648
  2147483649 >> 0; // => -2147483647
  ```

<a name="coercion--booleans"></a><a name="22.6"></a>
- [22.6](#coercion--booleans) Для приведения к логическому типу используйте `!!` или `Boolean`, в зависимости от того, что лучше влияет на удобочитаемость.

  eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  >Почему: `new Boolean` создает объект-обертку, что повышает вероятность ошибок (`!!(new Boolean(false)) // true`)

  ```javascript
  const age = 0;

  // плохо
  const hasAge = new Boolean(age);

  // хорошо
  const hasAge = Boolean(age);

  // наилучший способ
  const hasAge = !!age;
  const filteredArr = arr.filter(Boolean);
  ```

**[К содержанию](#table-of-contents)**

## Именование <a name="naming"></a>

<a name="naming--descriptive"></a><a name="23.1"></a>
- [23.1](#naming--descriptive) Названия должны быть описательны, избегайте названий из одной буквы.

  eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

  ```javascript
  // плохо
  function q () {
    // ...
  }

  // хорошо
  function query () {
    // ...
  }
  ```

<a name="naming--camelCase"></a><a name="23.2"></a>
- [23.2](#naming--camelCase) Используйте `camelCase` при именовании объектов, функций и экземпляров классов.

  eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase)

  ```javascript
  // плохо
  const OBJect = {};
  const this_is_my_object = {};
  function c () {}

  // хорошо
  const thisIsMyObject = {};
  function thisIsMyFunction () {}
  ```

<a name="naming--PascalCase"></a><a name="23.3"></a>
- [23.3](#naming--PascalCase) Используйте `PascalCase` только при именовании конструкторов или классов.

  eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap)

  ```javascript
  // плохо
  function user (options) {
    this.name = options.name;
  }

  const bad = new user({
    name: 'nope'
  });

  // хорошо
  class User {
    constructor (options) {
      this.name = options.name;
    }
  }

  const good = new User({
    name: 'yup'
  });
  ```

<a name="naming--leading-underscore"></a><a name="23.4"></a>
- [23.4](#naming--leading-underscore) Не начинайте название с символа подчеркивания и не заканчивайте им.

  eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle)

  >Почему: в JavaScript нет модификаторов доступа к переменным и методам. Хотя, по ощему соглашению, символ подчеркивания в начале имени означает, что переменная `private`, на самом деле она `public` и выступает как часть `API`.

  ```javascript
  // плохо
  this.__firstName__ = 'Panda';
  this._firstName = 'Panda';
  this.firstName_ = 'Panda';

  // хорошо
  this.firstName = 'Panda';
  ```

<a name="naming--self-this"></a><a name="23.5"></a>
- [23.5](#naming--self-this) Не сохраняйте ссылки на `this`, используйте стрелочные функции или [Function#bind](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

  ```javascript
  // плохо
  function foo () {
    const self = this;
    return function () {
      console.log(self);
    };
  }

  // плохо
  function foo () {
    const that = this;
    return function () {
      console.log(that);
    };
  }

  // хорошо
  function foo () {
    return () => {
      console.log(this);
    };
  }
  ```

<a name="naming--filename-matches-export"></a><a name="23.6"></a>
- [23.6](#naming--filename-matches-export) Имя файла должно соответствовать тому, что из него экспортируется.

  ```javascript
  // плохо
  // файл Radio.js
  export class Checkbox {
    // ...
  }

  // хорошо
  // файл Checkbox.js
  export class Checkbox {
    // ...
  }
  ```

<a name="naming--PascalCase-singleton"></a><a name="23.7"></a>
- [23.7](#naming--PascalCase-singleton) Используйте `PascalCase` при экспорте конструктора, класса, `singleton`-а, библиотеки функций, простого объекта.

  ```javascript
  export const PortalStyleGuide = {
    es6: {
    },
  };
  ```

<a name="naming--Acronyms-and-Initialisms"></a><a name="23.8"></a>
- [23.8](#naming--Acronyms-and-Initialisms) Акронимы и аббревиатуры должны быть все в верхнем или все в нижнем регистре.

  >Почему: для удобочитаемости

  ```javascript
  // плохо
  import SmsContainer from './containers/SmsContainer';

  // плохо
  const HttpRequests = [
    // ...
  ];

  // хорошо
  import SMSContainer from './containers/SMSContainer';

  // хорошо
  const HTTPREquests = [
    // ...
  ];

  // тоже хорошо
  const httpRequests = [
    // ...
  ];

  // наилучший вариант
  import TextMessageContainer from './containers/TextMessageContainer';

  // наилучший вариант
  const requests = [
    // ...
  ];
  ```

<a name="naming--purpose-in-variable-name"></a><a name="23.9"></a>
- [23.9](#naming--purpose-in-variable-name) Наименование должно отражать суть (назначение) именуемой переменной, класса, метода в контексте приложения.

  >Почему: для удобочитаемости и упрощения масштабирования.

  ```javascript
  // плохо
  const def = {
    title: 'Default user',
    // ...
  };

  // хорошо
  const defaultUser = {
    title: 'Default user',
    // ...
  };

  // ужасно
  const a = someCheck();
  const ha = someCheck();
  
  // плохо
  const attribute = someCheck();
  
  // Допустимо, если константа инициализирована в контексте функционального блока до 10 строк.
  const hasAttribute = someCheck();

  // хорошо
  const isBusinessObjectHasAttribute = someCheck();

  // ужасно
  const cols = getColumns(usersTable);
  const width = calcTotal(cols, 'width');

  const cols2 = getColumns(colorsTable);
  const cols2Width = calcTotal(cols2, 'width');

  // хорошо
  const usersTableColumns = getColumns(usersTable);
  const usersTableTotalWidth = calcTotal(usersTableColumns, 'width');

  const colorsTableColumns = getColumns(colorsTable);
  const colorsTableTotalWidth = calcTotal(colorsTableColumns, 'width');
  ```

**[К содержанию](#table-of-contents)**

## Методы доступа <a name="accessors"></a>

<a name="accessors--not-required"></a><a name="24.1"></a>
- [24.1](#accessors--not-required) Методы доступа для свойств не обязательны.

<a name="accessors--no-getters-setters"></a><a name="24.2"></a>
- [24.2](#accessors--no-getters-setters) Не используйте методы доступа Javascript (`getter`-ы и `setter`-ы), т.к. они являются причиной неожиданных побочных эффектов, их сложнее тестировать, поддерживать и судить о них. Вместо этого, если вы делаете методы доступа, используйте `getVal()` и `setVal(value)`.

  ```javascript
  // плохо
  class Dragon {
    get age () {
      // ...
    }

    set age (value) {
      // ...
    }
  }

  // хорошо
  class Dragon {
    getAge () {
      // ...
    }

    setAge (value) {
      // ...
    }
  }
  ```

<a name="accessors--boolean-prefix"></a><a name="24.3"></a>
- [24.3](#accessors--boolean-prefix) Если свойство типа `boolean` или метод возвращается `boolean` используйте `isVal()` или `hasVal()`.

  ```javascript
  // плохо
  if (!dragon.age()) {
    return false;
  }

  // хорошо
  if (!dragon.hasAge()) {
    return false;
  }
  ```

<a name="accessors--consistent"></a><a name="24.4"></a>
- [24.4](#accessors--consistent) Нормально создавать функции `get()` и `set()`, но будьте последовательны.

  ```javascript
  class Jedi {
    constructor (options = {}) {
      const lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    set (key, value) {
      this[key] = value;
    }

    get (key) {
      return this[key];
    }
  }
  ```

**[К содержанию](#table-of-contents)**

## События <a name="events"></a>

<a name="events--hash"></a><a name="25.1"></a>
- [25.1](#events) При передаче данных в событие (неважно, обычное событие DOM или что-то более проприетарное типа событий в `Backbone`) используйте объект-литерал (также известный как "хэш") вместо обычного значения. Это позволит последующим участникам легко добавлять данные при передаче их в событие и  избавит от необходимости обновления всех обработчиков события. Например, вместо:

  ```javascript
  // плохо
  $(this).trigger('listingUpdated', listing.id);

  // ...

  $(this).on('listingUpdated', (e, listingId) => { /* действия с listingId */ });
  ```

  предпочтите:

  ```javascript
  // хорошо
  $(this).trigger('listingUpdated', {listingId: listing.id});

  // ...

  $(this).on('listingUpdated', (e, data) => { /* действия с data.listingId */ });
  ```

**[К содержанию](#table-of-contents)**

## jQuery <a name="jquery"></a>

<a name="jquery--dollar-prefix"></a><a name="26.1"></a>
- [26.1](#jquery--dollar-prefix) В названии переменной, содержащей объект jQuery, используйте префикс `$`.

  ```javascript
  // плохо
  const sidebar = $('.sidebar');

  // хорошо
  const $sidebar = $('.sidebar');
  ```

<a name="jquery--cache"></a><a name="26.2"></a>
- [26.2](#jquery--cache) Кешируйте результаты выполнения запросов jQuery.

  ```javascript
  // плохо
  function setSidebar () {
    $('.sidebar').hide();

    // ...

    $('.sidebar').css({
      'background-color': 'pink'
    });
  }

  // хорошо
  function setSidebar () {
    const $sidebar = $('.sidebar');
    $sidebar.hide();

    // ...

    $sidebar.css({
      'background-color': 'pink'
    });
  }
  ```

<a name="jquery--queries"></a><a name="26.3"></a>
- [26.3](#jquery--queries) Для запросов в DOM используйте каскад `$('.sidebar ul)` или или дочерние селекторы `$('.sidebar > ul')`.

<a name="jquery--find"></a><a name="26.4"></a>
- [26.4](#jquery--find) Используйте `find` для поиска в сохраненных объектах jQuery.

  ```javascript
  // плохо
  $('ul', '.sidebar').hide();

  // плохо
  $('.sidebar').find('ul').hide();

  // хорошо
  $('.sidebar ul').hide();

  // хорошо
  $('.sidebar > ul').hide();

  // хорошо
  $sidebar.find('ul').hide();
  ```

**[К содержанию](#table-of-contents)**

## Совместимость с ECMAScript 5 <a name="es5-compat"></a>

<a name="es5-compat--kangax"></a>
- [27.1](#es5-compat--kangax) См. [таблицу совместимости](https://kangax.github.io/es5-compat-table) пользователя [Kangax](https://twitter.com/kangax).

**[К содержанию](#table-of-contents)**

## Стиль написания ECMAScript 6+ (ES 2015+) <a name="es6-styles"></a>

<a name="es6-styles--links"></a><a name="28.1"></a>
- [28.1](#es6-styles--links) Список ссылок на разделы этого руководства, описывающие особенности ES6+.

1. [Стрелочные функции](#arrow-functions)
1. [Классы и конструкторы](#constructors)
1. [Сокращенный синтаксис методов объектов](#objects--es6-object-shorthand)
1. [Сокращенный синтаксис свойств](#objects--es6-object-concise)
1. [Вычисляемые имена свойств](#objects--es6-computed-properties)
1. [Строковые шаблоны](#strings--es6-template-literals)
1. [Деструктурирующее присваивание](#destructuring)
1. [Значения аргументов по умолчанию](#functions--es6-default-parameters)
1. [Rest-синтаксис](#functions--es6-rest)
1. [Оператор расширения в массивах](#arrays--es6-array-spreads)
1. [Let и const](#references)
1. [Оператор возведения в степень](#properties--es2016-expoenentiation-operator)
1. [Итераторы и генераторы](#iterators-and-generators)
1. [Модули](#modules)

<a name="es6-styles--tc39-proposals"></a><a name="28.2"></a>
- [28.2](#es6-styles--tc39-proposals) Не используйте [предложения TC39](https://gitgub.io/tc39/proposals), которые не достигли стадии 3.

  >Почему: [работа над ними не закончена](https://tc39.github.io/process-document) и они могут быть изменены или вовсе удалены. Фактически они не являются частью JavaScript.

**[К содержанию](#table-of-contents)**

## Стандартная библиотека <a name="standard-library"></a>

[Стандартная библиотека](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects) содержит утилиты, функциональность которых сломана. Они сохранены для обратной совместимости.

<a name="standard-library--isnan"></a><a name="29.1"></a>
- [29.1](#standard-library--isnan) Используйте `Number.isNan` вместо `isNan`.

  eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  >Почему: глобальня функция `isNaN` приводит не-числа к числам и возвращает `true` если аргумент приводится к `NaN`. Если нужно именно такое поведение, это должно быть очевидно.

  ```javascript
  // плохо
  isNaN('1.2'); // false
  isNaN('1.2.3'); // true

  // хорошо
  Number.isNaN('1.2.3'); // false
  Number.isNaN(Number('1.2.3')); // true
  ```

<a name="standard-library--isfinite"></a><a name="29.2"></a>
- [29.2](#standard-library--isfinite) Используйте `Number.isFinite` вместо `isFinite`.

  eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  >Почему: глобальня функция `isFinite` приводит не-числа к числам и возвращает `true` если аргумент приводится к конечному значению. Если нужно именно такое поведение, это должно быть очевидно.

  ```javascript
  // плохо
  isFinite('2e3'); // true

  // хорошо
  Number.isFinite('2e3'); // false
  Number.isFinite(parseInt('2e3', 10)); // true
  ```

**[К содержанию](#table-of-contents)**

## TypeScript <a name="typescript"></a>

<a name="typescript--types-vs-interfaces"></a><a name="30.1"></a>

- [30.1](#typescript--types-vs-interfaces) Используйте типы для описания данных и операций с ними, функций, "внутреннего" поведения, а интерфейсы - для гарантии реализации "внешнего" поведения и расширения имеющихся типов и интерфейсов. В затруднительных ситуациях предпочтение стоит отдать типам.

  >Почему: типы более понятны, а их возможности шире. Подробнее см. [почему стоит использовать типы вместо интерфейсов](../rationales/types-vs-interfaces.md).

  **MyComponent.types.ts**

  ```typescript
  export type OwnProps = {
    // OwnProps - тип потому, что обычно в свойствах передаются
    // данные или функции-обработчики событий.
  }

  export type ConnectedProps = {
    // ConnectedProps - тип потому, что тут всегда данные.
  }

  export interface IBehaviorName {
    // IBehaviorName - интерфейс потому, что тут добавляется "внешнее"
    // поведение компоненту, который, в теории, можно заменить другим
    // компонентом, реализующим тот же интерфейс
  }

  export type Props = OwnProps & ConnectedProps & IBehaviorName
  ```

  **selectors.ts**

  ```typescript
  const props = (state: AppState): ConnectedProps => ({
    /* ... */
  })

  const functions: IBehaviorName = {
    function1,
    function2,
    // ...
  }
  ```

**[К содержанию](#table-of-contents)**
