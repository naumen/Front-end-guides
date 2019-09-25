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
	- [Использование JavaScript-селекторов](#using-javascript-hooks)
	- [Переопределение](#override)
	- [Каскад](#cascade)
	- [Комментарии](#comments)
	- [Пробельные символы и пустые строки](#whitespace)
	- [css-переменные](#custom-properties)
	- [БЭМ](#css-bem)
	- [Сначала мобильные](#mobile-first)
	- [Использование медиа-запросов](#media-queries)
1. [Less](#less)
	- [Общие положения](#less-common-terms)
	- [Вложенность](#nesting)
	- [Переменные](#variables)
	- [Функции](#functions)
	- [БЭМ](#less-bem)
	- [Именование селекторов](#naming)

## Термины <a name="terms"></a>

### Правила <a name="terms-rules"></a>

Правилом считаем блок, состоящий из селектора или группы селекторов и связанной с ними группы свойств и значений.

```css
a,
.link {
	text-decoration: none;
}

```

### Селекторы <a name="terms-selectors"></a>

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

### Свойства <a name="terms-properties"></a>

Свойства определяют какие характеристики целевого элемента необходимо изменить.

```css
/* селектор */ {
	/* меняется цвет фона */
	background-color: #EAEAEA; 
}
```

### Значения <a name="terms-values"></a>

Значения определяют как именно меняется указанная характеристика.

```css
/* селектор */ {
	/* цвет фона меняется на #EAEAEA */
	background-color: #EAEAEA; 
}
```

**[К содержанию](#table-of-contents)**
