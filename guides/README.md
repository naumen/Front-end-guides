# Руководство по стилю кода JavaScript

Другие руководства

- [React]

## Содержание
<a name="table-of-contents"></a>
1. [Типы]
1. [Ссылки]
1. [Объекты]
1. [Массивы]
1. [Деструктурирующее присваивание](#destructuring)
1. [Строки](#strings)
1. [Функции](#functions)
1. [Стрелочные функции](#arrow-functions)
1. [Классы и конструкторы]
1. [Модули]
1. [Итераторы и генераторы]
1. [Свойства]
1. [Переменные](#variables)
1. [Подъем переменных]
1. [Операторы сравнения и равенства]

## Деструктурирующее присваивание
<a name="destructuring"></a>
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

## Строки
<a name="strings"></a>
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
- [6.3](#strings--es6-template-literals) При программной генерации строк используйте строковые шаблоны вместо конкатенации.

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

## Функции
<a name="functions"></a>
<a name="functions--signature-spacing"></a><a name="7.1"></a>
- [7.1](#functions--signature-spacing) Пробельные символы в сигнатуре функции

  eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren), [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks) [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing)
  
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

## Стрелочные функции
<a name="arrow-functions"></a>
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
- [8.2](#arrow-functions--implicit-return) Если тело функции состоит из одной строки, которая возвращает [выражение](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) без побочных эффектов, опустите фигурные скобки и используйте неявный `return`.

  eslint: [`arrow-parens`](http://esling.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.og/docs/rules/arrow-body-style.html)

  >Почему: легко читается, когда несколько функций образуют цепочку

  ```javascript
  // плохо
  [1, 2, 3].map(number => {
    const nextNumber = number + 1;
    `A string containing the ${nextNumber}`;
  })

  // хорошо
  [1, 2, 3].map(number => `A string containing the ${number}`)

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

**[К содержанию](#table-of-contents)**

## Переменные

<a name="variables--const"></a><a name="1.1"></a>
- [13.1](#variables--const) При объявлении переменных всегда используйте `const` или `let`.

  eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)

  >Почему: использование `var` приводит к глобальным переменным и замусориванию глобальной области видимости

  ```javascript
  // плохо
  var persons = ['Almalexia', 'Sotha Sil', 'Vivec'];
  
  // хорошо
  const persons = ['Almalexia', 'Sotha Sil', 'Vivec'];
  ```

**[К содержанию](#table-of-contents)**