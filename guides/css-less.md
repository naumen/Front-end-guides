# Руководство по стилю кода CSS/Less

Другие руководства:

- [по стилю кода JavaScript](javascript.md)
- [по стилю кода React/Redux](react.md)
- [по самообучению](frontend-roadmap.md)

## Содержание <a name="table-of-contents"></a>

1. [Термины](#terms)
    - [Правила](#terms-rules)
    - [Селекторы](#terms-selectors)
    - [Свойства](#terms-properties)
    - [Значения](#terms-values)
1. [CSS](#css)
    - [Общие положения](#css-common-terms)
    - [Форматирование](#formatting)
    - [Упорядочение](#sorting)
    - [Использование ID](#using-ids)
    - [Переопределение](#override)
    - [Каскад](#cascade)
    - [Комментарии](#comments)
    - [css-переменные](#custom-properties)
    - [Именование селекторов](#css-naming)
    - [Сначала мобильные](#mobile-first)
    - [Использование медиа-запросов](#media-queries)
    - [Использование JavaScript-хуков](#using-javascript-hooks)
1. [Less](#less)
    - [Общие положения](#less-common-terms)
    - [Именование селекторов](#less-naming)
    - [Вложенность](#nesting)
    - [Переменные](#variables)
    - [Функции](#functions)

## Термины <a name="terms"></a><a name="1"></a>

### Правила <a name="terms-rules"></a><a name="1.1"></a>

Правилом считаем блок, состоящий из селектора или группы селекторов и связанной с ними группы свойств и значений.

```css
a,
.link {
  text-decoration: none;
}

```

### Селекторы <a name="terms-selectors"></a><a name="1.2"></a>

Селекторы определяют, к каким элементам `DOM` будут применены стили, задаваемые блоком свойств и значений, идущим сразу после селектора.

```css
a {
  /* свойства и значения */
}

.link {
  /* свойства и значения */
}

[type=checkbox] {
  /* свойства и значения */
}
```

### Свойства <a name="terms-properties"></a><a name="1.3"></a>

Свойства определяют какие характеристики целевого элемента необходимо изменить.

```css
/* селектор */ {
  /* меняется цвет фона */
  background-color: #EAEAEA;
}
```

### Значения <a name="terms-values"></a><a name="1.4"></a>

Значения определяют как именно меняется указанная характеристика.

```css
/* селектор */ {
  /* цвет фона меняется на #EAEAEA */
  background-color: #EAEAEA;
}
```

**[К содержанию](#table-of-contents)**

## CSS <a name="css"></a><a name="2"></a>

### Общие положения <a name="css-common-terms"></a><a name="2.1"></a>

Мы считаем, что написание кода должно приносить удовольствие. Поэтому мы:

- стараемся писать чистый, легко читаемый, понятный код, который одинаково легко воспринимается, изменяется и дополняется разработчиками с разным опытом и знаниями;
- не используем «хаки» и сложный синтаксис;
- создаем масштабируемые и гибкие решения;
- пишем безопасный код;
- работаем с мыслью о производительности;
- прикладываем усилия, чтобы написанный нами код работал на широком спектре устройств (в пределах разумного).

Мы понимаем, что бывают спорные и сложные случаи, когда применение правил этого руководства невозможно или нежелательно. Поэтому мы допускаем отступления от этих правил в пределах разумного.

### Форматирование <a name="formatting"></a><a name="2.2"></a>

<a name="formatting--one-selector-or-property-per-line"></a><a name="2.2.1"></a>
- [2.2.1](#formatting--one-selector-or-property-per-line) Один селектор в строке, одно свойство в строке.

  >Почему: легче читать, проще `diff`-ы.

  ```css
  /* плохо */
  a, a:hover, a:focus {
    /* ... */
  }

  a {color: #085896; text-decoration: underline;}

  /* хорошо */
  a,
  a:hover,
  a:focus {
    /* ... */
  }

  a {
    color: #085896;
    text-decoration: underline;
  }
  ```

<a name="formatting--braces"></a><a name="2.2.2"></a>
- [2.2.2](#formatting--braces) При оформлении отступов и скобок используем вариант [1TBS](https://en.wikipedia.org/wiki/Indentation_style#Variant:_1TBS_(OTBS)).

  >Почему: экономия строк; блок свойств и значений легче считывается глазами.

  ```css
  /* плохо */
  .container
  {
    /* ... */
  }

  .container
  { /* ... */ }

  .container {
    /* ... */
    }

  /* хорошо */
  .container {
    /* ... */
  }
  ```

<a name="formatting--indentation"></a><a name="2.2.3"></a>
- [2.2.3](#formatting--indentation) Для отступов используем символ табуляции.

  >Почему: в `css` нужны отступы, их удобнее делать при помощи симовлов табуляции, размер которых можно настроить под себя. Выравнивание, которое удобнее делать пробелами, наоборот требуется реже.

<a name="formatting--vertical-indentation"></a><a name="2.2.4"></a>
- [2.2.4](#formatting--vertical-indentation) Правила разделяем пустой строкой, свойства не разделяем.

  >Почему: разделение правил пустой строкой улучшает читаемость; свойства стоит разделять пустой строкой при их группировке, но мы используем сортировку по алфавиту для свойств.

  ```css
  /* плохо */
  .container {
    display: flex;
  }
  .block {
    background-color: #FAFAFA;
    border: 1px solid #333;
    color: #333;

    flex-grow: 0;
    flex-shrink: 1;
    flex-basis: auto;

    padding: 10px;
  }

  /* хорошо */
  .container {
    display: flex;
  }

  .block {
    background-color: #FAFAFA;
    border: 1px solid #333;
    color: #333;
    flex-basis: auto;
    flex-grow: 0;
    flex-shrink: 1;
    padding: 10px;
  }
  ```

<a name="formatting--spaces"></a><a name="2.2.5"></a>
- [2.2.5](#formatting--spaces) Отделяем значения от свойств и между собой одним пробелом, перед символом двоеточия пробел не ставим.

  >Почему: для удобочитаемости.

  ```css
  /* плохо */
  .block {
    box-shadow: 5px 5px 0px 0px #289FED,10px 10px 0px 0px #5FB8FF;
    margin : 0 20px;
    padding: 10px 0
  }

  .block {
    margin:  0 20px;
    padding: 10px 0
  }

  /* хорошо */
  .block {
    box-shadow: 5px 5px 0px 0px #289FED, 10px 10px 0px 0px #5FB8FF;
    margin: 0 20px;
    padding: 10px 0
  }

  .block {
    margin: 0 20px;
    padding: 10px 0
  }
  ```

<a name="formatting--newlines-in-values"></a><a name="2.2.6"></a>
- [2.2.6](#formatting--newlines-in-values) Допускается разбиение сложных значений по строкам.

  >Почему: легче читать, проще `diff`-ы.

  ```css
  .block {
    box-shadow: 5px 5px 0px 0px #289FED,
      10px 10px 0px 0px #5FB8FF;
  }

  .block {
    box-shadow:
      5px 5px 0px 0px #289FED,
      10px 10px 0px 0px #5FB8FF;
  }
  ```

<a name="formatting--semicolons"></a><a name="2.2.7"></a>
- [2.2.7](#formatting--semicolons) Всегда используем точку с запятой после значения, даже когда в блоке только одно свойство.

  >Почему: ради единообразного и предсказуемого кода; проще `diff`-ы; проще добавлять новые свойства.

  ```css
  /* плохо */
  .container {
    position: relative
  }

  .block {
    position: absolute;
    z-index: 1
  }

  /* хорошо */
  .container {
    position: relative;
  }

  .block {
    position: absolute;
    z-index: 1;
  }
  ```

### Упорядочение <a name="sorting"></a><a name="2.3"></a>

<a name="sorting--selectors"></a><a name="2.3.1"></a>
- [2.3.1](#sorting--selectors) Селекторы не сортируем, за исключением псевдоклассов `link`, `visited`, `hover`, `focus` и `active`. Селекторы группируем по назначению.

  >Почему: неправильная сортировка псевдоклассов приводит к неправильной последовательности применения стилей; группировка селекторов улучшает читаемость.

  ```css
  /* плохо */
  a:active,
  a:hover,
  a:focus {
    /* ... */
  }

  a:link,
  a:visited {
    /* ... */
  }

  a:link,
  .link,
  a:visited,
  .link_visited {
    /* ... */
  }

  /* хорошо */
  a:link,
  a:visited {
    /* ... */
  }

  a:hover,
  a:active,
  a:focus {
    /* ... */
  }

  a:link,
  a:visited,
  .link,
  .link_visited {
    /* ... */
  }
  ```

<a name="sorting--natural"></a><a name="2.3.2"></a>
- [2.3.2](#sorting--natural) В файле стилей используем «естественную» сортировку: правила появляются в том же порядке, что и в коде и группируются по назначению.

  >Почему: подобная сортировка позволяет легче ориентироваться в коде; группировка позволяет не размазывать код по файлу.

  При данном `html`-коде:

  ```html
  <button class="button">
    <span class="button-icon button-icon_ok">
      <img src="images/ok.png" alt="" />
    </span>
    <span class="button-caption">Button</span>
  </button>
  ```

  `css`-код должен выглядеть примерно так:

  ```css
  .button {
    /* ... */
  }

  .button-icon {
    /* ... */
  }

  /* варианты иконок сгруппированы */
  .button-icon_ok {
    /* ... */
  }

  .button-icon_whatever {
    /* ... */
  }

  .button-caption {
    /* ... */
  }
  ```

<a name="sorting--props-alpha"></a><a name="2.3.3"></a>
- [2.3.3](#sorting--props-alpha) Свойства сортируем по алфавиту, кроме исключений, описанных в п. [2.3.4](#sorting--ordered-props).

  >Почему: в больших блоках легче найти нужное свойство, если свойства отсортированы по алфавиту; в маленьких блоках достигается единообразие и предсказуемость кода.

- [2.3.4](#sorting--ordered-props) При сортировке свойств исключение составляют «развернутые» из коротких форм свойства. Они сортируются в том порядке, в котором происходит обход значений в короткой форм.

  Свойства-исключения: `border`, `margin`, `padding`.

  >Почему: так легче пропустить нужное свойство.

  ```css
  /* плохо */
  .container {
    padding-bottom: 7px;
    padding-left: 10px;
    padding-right: 10px;
    padding-top: 5px;
  }

  /* хорошо */
  .container {
    padding-top: 5px;
    padding-right: 10px;
    padding-bottom: 7px;
    padding-left: 10px;
  }
  ```

### Использование ID <a name="using-ids"></a><a name="2.4"></a>

<a name="using-ids--avoid-until-absolutely-necessary"></a><a name="2.4.1"></a>
- [2.4.1](#using-ids--avoid-until-absolutely-necessary) Не используем идентификаторы для стилизации до тех пор, пока это не является абсолютно необходимым.

  >Почему: использование идентификаторов приводит к созданию кода, который невозможно переиспользовать и сложно переопределять.

### Переопределение <a name="override"></a><a name="2.5"></a>

<a name="override--avoid"></a><a name="2.5.1"></a>
- [2.5.1](#override--avoid) Стараемся доопределять правила новыми свойствами, а не переопределять уже существующие свойства.

  >Почему: доопределение дает больше контроля над кодом; переопределение же приводит к более хрупкому коду, более специфичным селекторам, лишним перерисовкам.

  ```css
  /* плохо */
  .button {
    background-color: #F5F5F5;
    /* ... */
  }

  .button_negative {
    background-color: #EB5757;
    /* ... */
  }

  .button_positive {
    background-color: #4D92C8;
    /* ... */
  }

  /* хорошо */
  .button {
    /* ... */
  }

  .button_default {
    background-color: #F5F5F5;
    /* ... */
  }

  .button_negative {
    background-color: #EB5757;
    /* ... */
  }

  .button_positive {
    background-color: #4D92C8;
    /* ... */
  }
  ```

<a name="override--avoid-important"></a><a name="2.5.2"></a>
- [2.5.2](#override--avoid-important) Если использование переопределения неизбежно, стараемся не использовать модификатор `!important`.

  >Почему: использование `!important` создает сложности в случаях, когда переопределение необходимо (например, частная сборка под определенные цели) и в целом может говорить о плохо структурированном коде.  

### Использование JavaScript-хуков <a name="using-javascript-hooks"></a>


**[К содержанию](#table-of-contents)**

## Less <a name="less"></a>

### Общие положения <a name="less-common-terms"></a>

**[К содержанию](#table-of-contents)**