# Руководство по стилю кода JavaScript

Другие руководства

- [React]

## Содержание <a name="table-of-contents"></a>

0. [Общие положение](#general)
1. [Типы]
1. [Ссылки]
1. [Объекты](#objects)
1. [Массивы](#arrays)
1. [Деструктурирующее присваивание](#destructuring)
1. [Строки](#strings)
1. [Функции](#functions)
1. [Стрелочные функции](#arrow-functions)
1. [Классы и конструкторы]
1. [Модули](#modules)
1. [Итераторы и генераторы]
1. [Свойства]
1. [Переменные](#variables)
1. [Подъем переменных]
1. [Операторы сравнения и равенства]

## Общие положения <a name="general"></a>

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
  }
  // хорошо
  const atom = {
    value: 1,
    addValue (value) {
      return atom.value + value;
    }
  }
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
- [6.1](#strings--qiotes) Используйте одинарные кавычки (`''`) для строк

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

## Переменные <a name="variables"></a>

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

**[К содержанию](#table-of-contents)**