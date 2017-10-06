# Руководство по стилю кода JavaScript

Другие руководства

- [React]

## Содержание

1. [Типы]
1. [Ссылки]
1. [Объекты]
1. [Массивы]
1. [Деструктурирующее присваивание]
1. [Строки]
1. [Функции](#functions)
1. [Стрелочные функции]
1. [Классы и конструкторы]
1. [Модули]
1. [Итераторы и генераторы]
1. [Свойства]
1. [Переменные](#variables)
1. [Подъем переменных]
1. [Операторы сравнения и равенства]

## Функции
<a name="functions--signature-spacing"></a><a name="7.1"></a>
- [7.1](#functions--signature-spacing) Пробельные символы в сигнатуре функции

  eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks) [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing)
  
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