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
    - [Комментарии](#comments)
    - [Каскад](#cascade)
    - [Именование селекторов](#css-naming)
    - [Сначала мобильные](#mobile-first)
    - [Использование медиа-запросов](#media-queries)
    - [Возможности CSS 3](#css-3)
    - [Использование JavaScript-хуков](#using-javascript-hooks)
    - [Использование сокращенного синтаксиса свойств](#using-shorthand-properties)
1. [Less](#less)
    - [Общие положения](#less-common-terms)
    - [Форматирование](#formatting-less)
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
    box-shadow: 5px 5px 0 0 #289FED,10px 10px 0 0 #5FB8FF;
    margin : 0 20px;
    padding: 10px 0
  }

  .block {
    margin:  0 20px;
    padding: 10px 0
  }

  /* хорошо */
  .block {
    box-shadow: 5px 5px 0 0 #289FED, 10px 10px 0 0 #5FB8FF;
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
    box-shadow: 5px 5px 0 0 #289FED,
      10px 10px 0 0 #5FB8FF;
  }

  .block {
    box-shadow:
      5px 5px 0 0 #289FED,
      10px 10px 0 0 #5FB8FF;
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

### Комментарии <a name="comments"></a><a name="2.6"></a>

<a name="comments--for-clarification-and-strucutre"></a><a name="2.6.1"></a>
- [2.6.1](#comments--for-clarification-and-strucutre) Используем комментарии для пояснений и задания структуры, если это необходимо. В файлах стилей не должно быть закомментированных блоков кода.

  >Почему: для чистоты кода; эксперименты должны храниться в ветках, а не в комментариях.

<a name="comments--separate-lines-for-long"></a><a name="2.6.2"></a>
- [2.6.2](#comments--separate-lines-for-long) Для длинных комментариев используем отдельные строки.

  >Почему: длинные комментарии зачастую отображают важную информацию в хрупких участках кода, поэтому нужно, чтобы они легко считывались и при их изменении были минизирован риск изменения кода.

  ```css
  /* плохо */
  .block {
    position: relative; /* See bug: http://issue-tracker.company.com/issue/13720086692401465346#issue-description */
  }

  /* хорошо */
  .block {
    /* See bug: http://issue-tracker.company.com/issue/13720086692401465346#issue-description */
    position: relative;
  }

  .block {
    /*
      See bugs:
        http://issue-tracker.company.com/issue/13720086692401465346#issue-description
        http://issue-tracker.company.com/issue/49565952254494720471#issue-description
    */
    position: relative;
  }
  ```

<a name="comments--end-of-line"></a><a name="2.6.3"></a>
- [2.6.3](#comments--end-of-line) Для кратких комментариев допустимо использовать строки с кодом.

  >Почему: краткие комментарии на отдельной строке посреди кода ухудшают читаемость.

  ```css
  /* плохо */
  .block {
    flex-basis: auto;
    flex-grow: 0;
    /* IE */
    flex-shrink: 1;
  }

  /* хорошо */
  .block {
    flex-basis: auto;
    flex-grow: 0;
    flex-shrink: 1; /* IE */
  }
  ```

<a name="comments--setting-structure"></a><a name="2.6.4"></a>
- [2.6.4](#comments--setting-structure) Комментарии для задания структуры должны помещаться на отдельные строки.

  >Почему: хотя в целом надо стремиться к модульности и избегать необходимости задания структуры в большом файле стилей, не исключены случаи когда надо задавать структуру. Такие «структурные» комментарии должны легко считываться.

  ```css
  /*
    Radio buttons
  */
  [type="radio"] {
    /* ... */
  }

  /* ниже описаны правила для радиокнопок */
  ```

### Каскад <a name="cascade"></a><a name="2.7"></a>

<a name="cascade--prefer-avoid"></a><a name="2.7.1"></a>
- [2.7.1](#cascade--prefer-avoid) Стараемся избегать каскада.

  >Почему: правило и `html`-код будут сильно связаны, что приводит к хрупкому решению; специфичность селектора может оказаться слишком велика; получается код, который нельзя переиспользовать.

### Именование селекторов <a name="css-naming"></a><a name="2.8"></a>

<a name="css-naming--bem"></a><a name="2.8.1"></a>
- [2.8.1](#css-naming--bem) Используем [соглашения по именованию](https://ru.bem.info/methodology/naming-convention/#%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0-%D1%84%D0%BE%D1%80%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-%D0%B8%D0%BC%D0%B5%D0%BD), предложенные в БЭМ. Мы не настаиваем на использовании нижнего регистра, дефисов и символов подчеркивания, но не используем `snake_case` для именования блоков, элементов и модификаторов. Важно, чтобы выбранный способ был постоянен.

  >Почему: такой подход позволяет создавать изолированные блоки; БЭМ имеет низкий порог вхождения.

  ```css
  /* плохо */
  .block_name-element_name__mod_name {
    /* ... */
  }

  /* хорошо */
  .blockName-elementName_modName {
    /* ... */
  }


  .BlockName_elementName--modName {
    /* ... */
  }

  /* наилучший вариант */
  .block-name__element-name_mod-name {
    /* ... */
  }
  ```

<a name="css-naming--reasonable-names"></a><a name="2.8.2"></a>
- [2.8.2](#css-naming--reasonable-names) Используем разумный подход в выборе названий селекторов: без аббревиатур (акронимы допустимы), корректные слова на английском, не слишком длинные, абстрактные.

  >Почему: такой подход улучшает читаемость и переиспользуемость кода; выбор правильных абстракций помогает писать не-`css`-код лучше.

### Сначала мобильные <a name="mobile-first"></a><a name="2.9"></a>

<a name="mobile-first--code"></a><a name="2.9.1"></a>
- [2.9.1](#mobile-first--code) Пишем код сначала для мобильных, постепенно дополняя его для отображения на устройствах с большими экранами.

  >Почему: меньший объем кода, более эффективный код, более быстрая разработка.

### Использование медиа-запросов <a name="media-queries"></a><a name="2.10"></a>

<a name="media-queries--limit-breakpoints-amount"></a><a name="2.10.1"></a>
- [2.10.1](#media-queries--limit-breakpoints-amount) Ограничиваем количество точек останова и не создаем новые без особой необходимости.

  >Почему: большое количество точек останова сложно поддерживать.

<a name="media-queries--prefer-min-prefix"></a><a name="2.10.2"></a>
- [2.10.2](#media-queries--prefer-min-prefix) В медиа-запросах предпочитаем использовать префикс `min`: `min-height`, `min-width`.

  >Почему: с одной стороны этому способствует подход «сначала мобильные» через доопределение правил, с другой - так мы получаем более понятные точки останова: `640px` вместо `639px`, `1280px` вместо `1279px` и т.д.

### Возможности CSS 3 и выше <a name="css-3"></a><a name="2.11"></a>

<a name="css-3--use-when-possible"></a><a name="2.11.1"></a>
- [2.11.1](#css-3--use-when-possible) Используем где возможно. При этом стоит ориентироваться на ценность используемой возможности и на список поддерживаемых проектом браузеров. Например, если необходимо поддерживать `IE`, придется отказаться от `custom properties`, т.к. использование `custom properties` сделает код неработоспособным в `IE`. Но от использования свойства `filter` можно не отказываться, т.к. его неработоспособность в `IE` не затрагивает основную функциональность проекта.

  >Почему: во имя прогресса.

### Использование JavaScript-хуков <a name="using-javascript-hooks"></a><a name="2.12"></a>

<a name="using-javascript-hooks--avoid"></a><a name="2.12.1"></a>
- [2.12.1](#using-javascript-hooks--avoid) Не создавайте правил, где именем селектора служит JavaScript-хук. Используйте префикс `.js` для именования JavaScript-хуков.

  >Почему: JavaScript-хуки используются для обеспечения работы компонентов. Стилизованные хуки тяжело поддерживать.

  ```css
  /* никогда */
  .js-carousel-item {
    /* ... */
  }
  ```

**[К содержанию](#table-of-contents)**

### Использование сокращенного синтаксиса свойств <a name="using-shorthand-properties"></a><a name="2.13"></a>

<a name="using-shorthand-properties--selectively"></a><a name="2.13.1"></a>
- [2.13.1](#using-shorthand-properties--selectively) Выборочно используем сокращенный синтаксис свойств.

  >Почему: сокращенный синтаксис некоторых свойств достаточно сложен; его использование препятствует лучшему пониманию `CSS`; в динамике может приводить к лишним перерисовкам и перекомпоновкам.

  Свойства, для которых решили использовать сокращенный синтаксис:
  - `background`,
  - `background-position`,
  - `border`,
  - `border-block`,
  - `border-block-end`,
  - `border-block-start`,
  - `border-{side}`,
  - `border-radius`,
  - `border-width`,
  - `columns`,
  - `inset`,
  - `margin`,
  - `outline`,
  - `overflow`,
  - `padding`,
  - `padding-block`,
  - `padding-inline`,
  - `text-decoration`,
  - `text-emphasis`,
  - `-webkit-text-stroke`,
  - `transition`.

  Примеры допустимых форм записи:

  ```css
  .someClassName {
    background: #639;
    background: #639 url(image.jpg);
    background: #639 url(image.jpg) no-repeat;
    background: #639 url(image.jpg) no-repeat 0 0;
    background: url(image.jpg);
    background: url(image.jpg) no-repeat;
    background: url(image.jpg) no-repeat 0 0;

    background-position: 50% 50%;

    border: 10px solid #639;

    border-bottom: 10px solid #639;
    border-left: 10px solid #639;
    border-right: 10px solid #639;
    border-top: 10px solid #639;

    border-block: 3px;
    border-block: 3px solid #639;

    border-block-end: 3px;
    border-block-end: 3px solid #639;

    border-block-start: 3px;
    border-block-start: 3px solid #639;

    border-radius: 10px;
    border-radius: 10px 5px;
    border-radius: 10px 5px 8px 6px;

    border-width: 10px;
    border-width: 10px 5px;
    border-width: 10px 5px 8px 6px;

    columns: 2 200px;

    inset: 10px;
    inset: 10px 15px;
    inset: 10px 15px 20px 25px;

    margin: 10px;
    margin: 10px 5px;
    margin: 10px 5px 8px 6px;

    outline: 2px solid #639;

    overflow: hidden;

    padding: 10px;
    padding: 10px 5px;
    padding: 10px 5px 8px 6px;

    padding-block: 10px;
    padding-block: 10px 20px;

    padding-inline: 10px;
    padding-inline: 10px 20px;

    text-decoration: underline;
    text-decoration: underline wavy;
    text-decoration: underline dotted #639;

    text-emphasis: filled #639;
    text-emphasis: filled double-circle #639;
    text-emphasis: 'x';

    -webkit-text-stroke: 2px #639;

    transition: top 3s ease-in-out;
    transition: top 3s ease-in-out 1s;
    transition: display 3s allow-discrete;
  }
  ```

  Использование сокращенного синтаксиса не обязательна, разработчик волен сам решать сокращенную или полную форму использовать в своем коде.

  Свойства, для которых мы не используем сокращенный синтаксис:
  - `animation`,
  - `column-rule`,
  - `flex`,
  - `font`,
  - `list-style`.

  Также не используем формы сокращенного синтаксиса, когда из 4 необходимых значений допустимо указывать 3, или когда указание 4 значений избыточно.

  На примере `border-width`, но применимо и к другим случаям:

  ```css
  .someClassName {
    border-width: 10px 5px 8px; /* 3 значения */
    border-width: 10px 5px 10px 5px; /* избыточное указание всех значений */
  }
  ```

**[К содержанию](#table-of-contents)**

## Less <a name="less"></a><a name="3"></a>

### Общие положения <a name="less-common-terms"></a><a name="3.1"></a>

Соглашения, описанные в этом разделе основаны на соглашениях о стиле кода `CSS` и дополняют их.

### Форматирование <a name="formatting-less"></a><a name="3.2"></a>

<a name="formatting-less--vertical-indentation"></a><a name="3.2.1"></a>
- [3.2.1](#formatting-less--vertical-indentation) Вложенные правила отделяем пустой строкой от свойств родительского правила.

  >Почему: для удобочитаемости.

  ```less
  /* плохо */
  a {
    text-decoration: underline;
    &:hover {
      text-decoration: none;
    }
  }

  /* хорошо */
  a {
    text-decoration: underline;

    &:hover {
      text-decoration: none;
    }
  }
  ```
  
### Именование селекторов <a name="less-naming"></a><a name="3.3"></a>

<a name="less-naming--no-bem"></a><a name="3.3.1"></a>
- [3.3.1](#less-naming--no-bem) Не используем БЭМ.

  >Почему: используем [`css`-модули](https://github.com/css-modules/css-modules).

<a name="less-naming--camel-case"></a><a name="3.3.2"></a>
- [3.3.2](#less-naming--camel-case) Записываем названия классов в селекторах только в `camelCase`.

  >Почему: это упрощает их использование, т.к. можно использовать точечную нотацию.

### Вложенность <a name="nesting"></a><a name="3.4"></a>

<a name="nesting--avoid-parent-selector-usage"></a><a name="3.4.1"></a>
- [3.4.1](#nesting--avoid-parent-selector-usage) Не используем «родительский селектор» для генерации названий классов.

  >Почему: `IDE` не распознает такие случаи и выдает предупреждения.

  ```less
  /* плохо */
  .button {
    /* ... */

    &Ok {
      /* ... */
    }

    &Cancel {
      /* ... */
    }
  }

  /* хорошо */
  .button {
    /* ... */
  }

  .buttonOk {
    /* ... */
  }

  .buttonCancel {
    /* ... */
  }
  ```

### Переменные <a name="variables"></a><a name="3.5"></a>

<a name="variables--reasonable"></a><a name="3.5.1"></a>
- [3.5.1](#variables--reasonable) Подходим разумно к использованию переменных:
    - выносим в переменные цвета, временные характеристики анимаций, значения свойств, связанных со шрифтами;
    - одно вхождение не повод вынести в переменную;
    - два вхождения - повод вынести в локальную переменную;
    - более двух вхождений - стоит вынести значение в отдельную переменнную в отдельный файл переменных.

  >Почему: чтобы сохранить рассудок

**[К содержанию](#table-of-contents)**
