# Руководство по стилю кода React/Redux

В основе этого документа лежат [руководство по стилю кода React Airbnb](https://github.com/airbnb/javascript/tree/master/react) и [руководство по стилю кода React/Redux iraycd](https://github.com/iraycd/React-Redux-Styleguide). Это руководство не является точным переводом и в нем есть отличия от оригиналов.

Другие руководства:

- [по стилю кода JavaScript](javascript.md)
- [по стилю кода CSS/Less](css-less.md)
- [по самообучению](frontend-roadmap.md)

## Содержание <a name="table-of-contents"></a>

1. [Базовые правила](#basic-rules)
1. [Компоненты](#components)
1. [Именование](#naming)
1. [Выравнивание](#alignment)
1. [Кавычки](#quotes)
1. [Пробельные символы](#spacing)
1. [Свойства](#props)
1. [Ссылки](#refs)
1. [Круглые скобки](#parentheses)
1. [Теги](#tags)
1. [Методы](#methods)
1. [Порядок в компоненте](#order-in-component)
1. [Redux](#redux)
1. [Утилиты и вспомогательные функции](#utils-and-helpers)
1. [Тестирование](#testing)

## Базовые правила <a name="basic-rules"></a>

<a name="basic-rules--component-types"></a><a name="1.1"></a>
- [1.1](#basic-rules--component-types) Разделяйте компоненты на контейнеры («умные») и представления («глупые»).

|&nbsp;          |контейнеры                   |представления                                               |
|:---------------|:----------------------------|:-----------------------------------------------------------|
|Где используются|в начале иерархии компонентов|в середине и конце иерархии компонентов                     |
|`Redux`         |знают о нем                  |не знают о нем                                              |
|Получают данные |из хранилища `Redux`         |из переданных свойств                                       |
|Меняют данные   |вбрасывают действия `Redux`  |вызывают функции обратного вызова, переданные через свойства|

  Такое разделение не является догмой. Иногда подобное разделение не имеет смысла. В других случаях сложно определить к какому типу относится компонент. В последнем случае, вероятно, еще рано делать разделение.

  Если нет возможности четко определить компонент как контейнер, следует отнести его к представлениям.

<a name="basic-rules--container-components"></a><a name="1.2"></a>
- [1.2](#basic-rules--container-components) При создании компонентов-контейнеров придерживайтесь следующих принципов:
  + контейнеры определяют логику работы;
  + могут содержать и контейнеры, и представления, но обычно не содержат никакой разметки, за исключением нескольких оборачивающих `div`-ов, и никогда не содержат стили;
  + предоставляют данные и поведение другим компонентам, как контейнерам, так и представлениям;
  + вызывают действия `Redux` и предоставляют их как функции обратного вызова компонентам-представлениям.

<a name="basic-rules--presentational-components"></a><a name="1.3"></a>
- [1.3](#basic-rules--presentational-components) При создании компонентов-представлений придерживайтесь следующих принципов:
  + представления отвечают за внешний вид приложения;
  + могут содержать и контейнеры, и представления, и имеют разметку и стили;
  + не зависят от остальных частей приложения, таких как действия `Redux` или состояние приложения;
  + не определяют, как данные загружаются или изменяются;
  + получают данные и функции обратного вызова только через свойства;
  + редко имеют свое состояние, а когда имеют - оно отражает состояние пользовательского интерфейса, а не содержит данные;
  + должны быть написаны как функциональные компоненты до тех пор, пока им не нужно состояние, методы жизненного цикла или оптимизация производительности.

<a name="basic-rules--component-folder"></a><a name="1.4"></a>
- [1.4](#basic-rules--component-folder) Организация файлов компонента: используйте подход `feature first`:
  + создайте две папки: одну для компонентов-контейнеров, другую для компонентов-представлений. Например `src/containers` и `src/components`.
  + изолируйте все, что относится к компоненту в одной папке, названной в соответствии с компонентом:

    ```text
    ...
    |  |-containers
    |  |  |-SomeFeatureContainer
    |  |  |  |-tests
    |  |  |  |  |-actions.test.js
    |  |  |  |  |-index.test.js
    |  |  |  |  \-reducer.test.js
    |  |  |  |-actions.js
    |  |  |  |-constants.js
    |  |  |  |-index.js
    |  |  |  |-styles.less
    |  |  |  |-reducer.js
    |  |  |  \-SomeFeatureContainer.jsx
    |  |  |
    |  |  |-YetAnotherFeatureContainer
    ...
    ```

  + папки компонентов могут содержать папки подкомпонентов, организованные тамик же образом, если эти подкомпоненты напрямую относятся к основному компоненту

  >Почему: лучшая переиспользуемость

**[К содержанию](#table-of-contents)**

## Компоненты <a name="components"></a>

<a name="components--no-multi-comp"></a><a name="2.1"></a>
- [2.1](#components--no-multi-comp) Один файл - один компонент.

  eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md)

  >Почему: объявление только одного компонента в файле улучшает читаемость и переиспользуемость компонента

  ```
  ...
  "react/no-multi-comp": [
    "error",
    {"ignoreStateless": false}
  ]
  ...
  ```

<a name="components--class-vs-react-create-class"></a><a name="2.2"></a>
- [2.2](#components--class-vs-react-create-class) Если в компоненте есть состояние и/или ссылки, используйте `class extends React.Component`; не используйте `React.createClass`.

  eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md)

  ```jsx
  // плохо
  const Listing = React.createClass({
    // ...

    render () {
      return <div>{this.state.hello}</div>;
    }
  });

  // хорошо
  class Listing extends React.Component {
    // ...

    render () {
      return <div>{this.state.hello}</div>;
    }
  };
  ```

<a href="components--class-vs-stateless"></a><a name="2.3"></a>
- [2.3](#components--class-vs-stateless) Если в компоненте нет состояния и ссылок, используйте обычную функцию, а не классы `es6`.

  eslint: [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

  ```jsx
  // плохо
  class Listing extends React.Component {
    render () {
      return <div>{this.props.hello}</div>;
    }
  }

  // хорошо
  const Listing = ({hello}) => <div>{hello}</div>;
  ```

<a href="components--extend-width-composition-or-hoc"></a><a name="2.4"></a>
- [2.4](#components--extend-width-composition-or-hoc) Расширяйте компоненты при помощи композиции или компонентов высшего порядка.
    + Композиция обеспечивается использованием `this.props.children`.

      ```jsx
      <Modal>
        <ModalTitle>Modal Title</ModalTitle>
        <ModalBody>
          Some content
        </ModalBody>
      </Modal>
      ```

    + Компонент высшего порядка - это функция, которая оборачивает существующий компонент в другой компонент. При этом могут выполняться следующие действия:
        - выполнение кода до и после вызова оборачиваемого компонента;
        - блокировка вывода оборачиваемого компонента по условию;
        - обновление свойств, передаваемых оборачиваемому компоненту и добавление новых свойств;
        - преобразование вывода оборачиваемого компонента (например, оборачивание в дополнительную разметку).

<a name="components--expose-component-state"></a><a name="2.5"></a>
- [2.5](#components--expose-component-state) Выставляйте внутреннее состояние компонента наружу через свойства и описания типов. Это позволит внешнему коду устанавливать начальное значение состояния и реагировать на изменения в компоненте.

  ```jsx
  // плохо
  class FlipCard extends Component {
    constructor (props) {
      super(props);

      this.state = {
        flipped: false
      };
    }

    toggleFlip = () => {
      this.setState({
        flipped: !this.state.flipped
      });
    };

    render () {
      const {flipped} = this.state;

      return (
        <div className={flipped ? 'showBack' : 'showFront'}>
          <sapn className="flipControl" onClick={this.toggleFlip} />
        </div>
      );
    }
  }

  // хорошо
  class FlipCard extends Component {
    constructor (props) {
      super(props);

      this.state = {
        flipped: props.flipped || false
      };
    }

    toggleFlip = () => {
      const {onFlip} = this.props;

      this.setState({
        flipped: !this.state.flipped
      }, () => onFlip && onFlip(this.state.flipped));
    };

    render () {
      const {flipped} = this.state;

      return (
        <div className={flipped ? 'showBack' : 'showFront'}>
          <sapn className="flipControl" onClick={this.toggleFlip} />
        </div>
      );
    }
  }
  ```

<a name="prefer-function-component"></a><a name="2.6"></a>
- [2.6](#prefer-function-component) Предпочитайте функциональные компоненты при создании новых и рефакторинге старых компонентов.

  >Почему: Функциональные компоненты лаконичнее, их проще писать и читать.

**[К содержанию](#table-of-contents)**

## Именование <a name="naming"></a>

<a name="naming--extensions"></a><a name="3.1"></a>
- [3.1](#naming--extensions) Используйте расширение `.jsx` для файлов, содержащих код `React`-компонентов.

<a name="naming--filenames"></a><a name="3.2"></a>
- [3.2](#naming--filenames) Используйте `PascalCase` для файлов, содержащих код `React`-компонентов.

<a name="naming--reference"></a><a name="3.3"></a>
- [3.3](#naming--reference) Используйте `PascalCase` для названий компонентов и `camelCase` для их экземпляров.

  eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

  ```jsx
  // плохо
  import formField from './FormField';

  // хорошо
  import FormField from './FormField';

  // плохо
  const FormField = <FormField />

  // хорошо
  const formField = <FormField />
  ```

<a name="naming--components"></a><a name="3.4"></a>
- [3.4](#naming--components) Используйте имя файла в качестве имени компонента. Не используйте `index.js` или `index.jsx` для кода корневых компонентов.

  >Почему: облегчает работу с `IDE`

  ```jsx
  // плохо
  import Footer from './Footer/Footer';

  // хорошо
  import Footer from './Footer';
  ```

<a name="naming--hoc"></a><a name="3.5"></a>
- [3.5](#naming--hoc) В генерируемых компонентах используйте `displayName`, чье значение состоит из названий компонента высшего порядка и переданного компонента. Например, если компонент высшего порядка называется `withFoo()`, то при передаче ему компонента `Bar` должен создаваться компонент с `displayName` `withFoo(Bar)`.

  >Почему: `displayName` компонента может использоваться в инструментах разработчика или в сообщениях об ошибках.

  ```jsx
  // плохо
  export default function withFoo(WrappedComponent) {
    return function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }
  }

  // хорошо
  export default function withFoo(WrappedComponent) {
    function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }

    const wrappedComponentName = WrappedComponent.displayName
      || WrappedComponent.name
      || 'Component';

    WithFoo.displayName = `withFoo(${wrappedComponentName})`;

    return WithFoo;
  }
  ```

<a name="naming--props"></a><a name="3.6"></a>
- [3.6](#naming--props) Избегайте использования названий свойств DOM для отличных от первоначальных целей.

  >Почему: ожидается, что свойства типа `style` или `className` имеют одну конкретную цель. Изменение этого `API` для своих целей делает код менее читабельным и поддерживаемым и может приводить к ошибкам.

  ```jsx
  // плохо
  <MyComponent style="fansy" />

  // плохо
  <MyComponent className="fancy" />

  // хорошо
  <MyComponent variant="fancy" />
  ```

**[К содержанию](#table-of-contents)**

## Выравнивание <a name="alignment"></a>

<a name="alignment--props"></a><a name="4.1"></a>
- [4.1](#alignment--props) Используйте следующие примеры для выравнивания свойств, закрывающей скобки и дочерних компонентов.

  eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md), [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

  ```jsx
  // плохо
  <Foo superLongParam="bar"
       anotherSuperLognParam="baz" />

  // хорошо
  <Foo
    superLongParam="bar"
    anotherSuperLognParam="baz"
  />

  // если свойства можно разместить в одну строку, стоит так сделать
  <Foo bar="bar" />

  // вложенные компоненты имеют нормальный отступ
  <Foo
      superLongParam="bar"
      anotherSuperLognParam="baz"
  >
    <Quux />
  </Foo>
  ```

**[К содержанию](#table-of-contents)**

## Кавычки <a name="quotes"></a>

<a name="quotes--types"></a><a name="5.1"></a>
- [5.1](#quotes--types) Используйте одинарные и двойные кавычки, а также машинописный обратный апостроф (`backtick`).

  >Почему: эти символы легко ввести с клавиатуры, независимо от типа используемой операционной системы. Хотя, разработчик чаще читает текст, чем его пишет и удобство чтения — важный аспект, мы пока не используем национальные кавычки (например, «ёлочки», ‟лапки” или “английские двойные”) по ряду причин:
  • сложно использовать, т.к. не каждый разработчик знает про третий и другие уровни клавиатуры;
  • сложно поддерживать консистентность использования в кодовой базе;
  • сложно искать текст средствами `IDE`, если в поисковой фразе должны быть кавычки;
  • сложно запомнить в каких случаях какие кавычки использовать.

<a name="quotes--single-vs-double"></a><a name="5.2"></a>
- [5.2](#quotes--single-vs-double) Используйте двойные кавычки для значений `html`- и `JSX`-атрибутов, одинарные для строковых значений и обратные для строк-шаблонов.

  eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

  >Почему: в `html` при задании значений атрибутов обычно используются двойные кавычки, в `JSX` стоит придерживаться этого же способа, т.к. он похож на `html` и `jsx`-код часто идет вперемежку с `html`-кодом; использование же одинарных кавычек для строковых значений в `javascript`-коде обеспечивает более чистый и читабельный код

  ```jsx
  // плохо
  <Foo bar='bar' />

  // хорошо
  <Foo bar="bar" />

  // плохо
  <Foo style={{ left: "20px" }} />

  // хорошо
  <Foo style={{ left: '20px' }} />
  ```

<a name="quotes--backticks-in-comments"><a name="5.3"></a>
- [5.3](#quotes--backticksin-comments) Используйте обратные апострофы в шаблонных строках и для визуального выделения аббревиатур, параметров функций, терминов в `md`-файлах, комментариях и `jsDoc`.

  >Почему: инструменты (например, `GitLab`) могут поддерживать визуальное выделение обернутых в обратные апострофы строк в `md`-файлах и комментариях, что облегчает чтение и понимание содержимого.

  ```javascript
  // плохо
  /**
   * ...
   * @param id - "uuid" объекта
   * ...
   */

  // нормально
  /**
   * ...
   * @param id - uuid объекта
   * ...
   */

  // хорошо
  /**
   * ...
   * @param id - `uuid` объекта
   * ...
   */
  ```

**[К содержанию](#table-of-contents)**

## Пробельные символы <a name="spacing"></a>

<a name="spacing--jsx-tag-spacing"></a><a name="6.1"></a>
- [6.1](#spacing--jsx-tag-spacing) Перед закрытием самозакрывающегося тега ставьте пробел.

  eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

  ```jsx
  // плохо
  <Foo/>

  // очень плохо
  <Foo          />

  // плохо
  <Foo
   />

  // хорошо
  <Foo />
  ```

<a name="spacing--curly-spacing"></a><a name="6.2"></a>
- [6.2](#spacing--curly-spacing) Не отбивайте фигурные скобки пробелами в `JSX`.

  eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

  ```jsx
  // плохо
  <Foo bar={ baz } />

  // хорошо
  <Foo bar={baz} />
  ```

**[К содержанию](#table-of-contents)**

## Свойства <a name="props"></a>

<a name="props--camelCase"></a><a name="7.1"></a>
- [7.1](#props--camelCase) Всегда используйте `camelCase` при именовании свойств.

  ```jsx
  // плохо
  <Foo
    UserName="John Doe"
    phone_number={123456789}
  />

  // хорошо
  <Foo
    userName="John Doe"
    phoneNumber={123456789}
  />
  ```

<a name="props--boolean-value"></a><a name="7.2"></a>
- [7.2](#props--boolean-value) Используйте полный синтаксис для свойств типа `boolean`.

  eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

  >Почему: для единообразия с синтаксисом остальных свойств

  ```jsx
  // плохо
  <Foo hidden />

  // хорошо
  <Foo hidden={true} />
  ```

<a name="props--alt"></a><a name="7.3"></a>
- [7.3](#props--alt) Всегда указывайте атрибут `alt` тега `img`. Если изображение презентационное, `alt` может содержать пустое значение или необходимо указать атрибут `role` со значением `presentation`.

  eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

  ```jsx
  // плохо
  <img src="hello.jpg" />

  // хорошо
  <img src="hello.jpg" alt="Me waving hello" />

  // хорошо
  <img src="hello.jpg" alt="" />

  // хорошо
  <img src="hello.jpg" role="presentation" />
  ```

<a name="props--aria-role"></a><a name="7.4"></a>
- [7.4](#props--aria-role) Используйте только правильные, не абстрактные [роли ARIA](https://www.w3.org/TR/wai-aria-1.1/#role_definitions).

  eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

  ```jsx
  // плохо - не ARIA-роль
  <div role="datepicker" />

  // плохо - абстрактная ARIA-роль
  <div role="range" />

  // хорошо
  <div role="button" />
  ```

<a name="props--no-access-key"></a><a name="7.5"></a>
- [7.5](#props--no-access-key) Не используйте атрибут `accessKey`.

  eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  >Почему: это приводит к ухудшению доступности для пользователей программ чтения с экрана

  ```jsx
  // плохо
  <div accessKey="h" />

  // хорошо
  <div />
  ```

<a name="props--no-index-as-a-key"></a><a name="7.6"></a>
- [7.6](#props--no-index-as-a-key) Не используйте индекс массива как значение свойства `key`, предпочитайте уникальный идентификатор.

  >Почему: [это антипаттерн](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)

  ```jsx
  // плохо
  {todos.map((todo, index) => <Todo {...todo} key={index} />)}

  // хорошо
  {todos.map(todo => <Todo {...todo} key={todo.id} />)}
  ```

<a name="props--default-props"></a><a name="7.7"></a>
- [7.7](#props--default-props) Всегда явно определяйте значения свойств по умолчанию для тех свойств, наличие которых не обязательно.

  >Почему: `propTypes` своего рода документация, а предоставление `defaultProps` избавит разработчика, который будет использовать этот код, от излишних предположений. Кроме того, это позволяет опустить некоторые проверки типов.

  ```jsx
  // плохо
  function SFC ({foo, bar, children}) {
    return <div>{foo}{bar}{children}</div>;
  }

  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node
  }

  // хорошо
  function SFC ({foo, bar, children}) {
    return <div>{foo}{bar}{children}</div>;
  }

  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node
  }

  SFC.defaultProps = {
    bar: '',
    children: null
  }
  ```

<a name="props--spread"></a><a name="7.8"></a>
- [7.8](#props--spread) Не злоупотребляйте оператором расширения.

  >Почему: в противном случае можно передать ненужные свойства компонентам. А для `React` версии `15.6.1` и старше вы можете [передать неправильные `HTML`-атрибуты в `DOM`](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html). 

  Исключения:

  - компоненты высшего порядка (`HOC`), которые передают свойства внутрь дочернего компонента и поднимают `propTypes`

    ```jsx
    function HOC (WrappedComponent) {
      return class Proxy extends React.Component {
        Proxy.propTypes = {
          text: PropTypes.string,
          isLoading: PropTypes.bool
        };

        render () {
          return <WrappedComponent {...this.props} />;
        }
      }
    }
    ```

  - при использовании оператора расширения для известных, явно заданных свойств. Это может быть особенно полезно при тестировании `React`-компонентов при помощи конструкции `beforeEach` `Mocha`.

    ```jsx
    export default function Foo {
      const props = {
        text: '',
        isPublished: false
      };

      return <div {...props} />;
    }
    ```
  Примечание к использованию: по возможности отфильтровывайте ненужные свойства, а также используйте [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) для профилактики ошибок.

  ```jsx
  // плохо
  render () {
    const {irrelevantProp, ...relevantProps} = this.props;
    return <WrappedComponent {...this.props} />;
  }

  // хорошо
  render () {
    const {irrelevantProp, ...relevantProps} = this.props;
    return <WrappedComponent {...relevantProps} />;
  }
  ```

**[К содержанию](#table-of-contents)**

## Ссылки <a name="refs"></a>

<a name="refs--no-string-refs"></a><a name="8.1"></a>
- [8.1](#refs--no-string-refs) Всегда используйте функции обратного вызова.

  eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

  ```jsx
  // плохо
  <Foo ref="myRef" />

  // хорошо
  <Foo ref={ref => this.myRef = ref} />
  ```

**[К содержанию](#table-of-contents)**

## Круглые скобки <a name="parentheses"></a>

<a name="parentheses--jsx-wrap-multilines"></a><a name="9.1"></a>
- [9.1](#parentheses--jsx-wrap-multilines) Оборачивайте `JSX`-теги в круглые скобки, когда они занимают несколько строк.

  eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

  ```jsx
  // плохо
  render () {
    return <MyComponent variant="long body" foo="bar">
             <MyContent />
           </MyComponent>;
  }

  // хорошо
  render () {
    return (
      <MyComponent variant="long body" foo="bar">
        <MyContent />
      </MyComponent>
    );
  }

  // хорошо, если теги умещаются в одну строку
  render () {
    const body = <div>Hello</div>;
    return <MyComponent>{body}</MyComponent>
  }
  ```

**[К содержанию](#table-of-contents)**

## Теги <a name="tags"></a>

<a name="tags--self-closing-comp"></a><a name="10.1"></a>
- [10.1](#tags--self-closing-comp) Используйте синтаксис самозакрывающихся тегов, если они не принимают другие теги в качестве дочерних.

  eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

  ```jsx
  // плохо
  <Foo variant="stuff"></Foo>

  // хорошо
  <Foo variant="stuff" />
  ```

<a name="tags--jsx-closing-bracker-location"></a><a name="10.2"></a>
- [10.2](#tags--jsx-closing-bracker-location) Если свойства компонента занимают несколько строк, закрывайте тег компонента на отдельной строке.

  eslint: [`react/jsx-closing-bracker-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

  ```jsx
  // плохо
  <Foo
    bar="bar"
    baz="baz" />

  // хорошо
  <Foo
    bar="bar"
    baz="baz"
  />
  ```

**[К содержанию](#table-of-contents)**

## Методы <a name="methods"></a>

<a name="methods--arrow-to-close-over"></a><a name="11.1"></a>
- [11.1](#methods--methods--arrow-to-close-over) Используйте стрелочные функции для замыкания локальных переменных.

  ```jsx
  function ItemList (props) {
    return (
      <ul>
        {props.items.map((item, index) => (
          <Item
            key={item.key}
            onClick={() => doSomethingWith(item.name, index)}
          />
        ))}
      </ul>
    );
  }
  ```

<a name="methods--arrow-vs-bind"></a><a name="11.2"></a>
- [11.2](#methods--arrow-vs-bind) Старайтесь не использовать [Function#bind](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind), используйте свойства класса и стрелочные функции.

  >Почему: более краткий синтаксис стрелочных функций. Грамотное использование `bind` легко может перейти в использование `bind` внутри методов отрисовки. В некоторых случаях единственной целью конструктора может оказаться передача контекста при помощи `bind`.

  ```jsx
  // плохо
  export class Foo extends Component {
    constructor (props) {
      super(props);

      this.onClick = this.onClick.bind(this);
    }

    onClick () {
      // do stuff
    }

    render () {
      return <div onClick={this.onClick)} />;
    }
  }

  // хорошо
  export class Foo extends Component {
    onClick = () => {
      // do stuff
    };

    render () {
      return <div onClick={this.onClick)} />;
    }
  }
  ```
  
<a name="methods--jsx-no-bind"></a><a name="11.2"></a>
- [11.3](#methods--jsx-no-bind) Если использовать `bind` необходимо, делайте это в конструкторе.

  eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  >Почему: вызов `bind` в методе `render` (и других методах отрисовки) создает новую функцию при каждой отрисовке

  ```jsx
  // плохо
  class Foo extends Component {
    onClick () {
      // do stuff
    }

    render () {
      return <div onClick={this.onClick.bind(this)} />;
    }
  }

  // хорошо
  class Foo extends Component<Props> {
    constructor (props) {
      super(props);

      this.onClick = this.onClick.bind(this);
    }

    onClick () {
      // do stuff
    }

    render () {
      return <div onClick={this.onClick)} />;
    }
  }
  ```

<a name="methods--no-underscore-for-internal"></a><a name="11.3"></a>
- [11.4](#methods--no-underscore-for-internal) Не используйте символ подчеркивания `_` в качестве префикса в названиях внутренних методов компонента.

  >Почему: префикс `_` иногда используется в других языках по соглашению для обозначения `private`-метода. В `JavaScript` нет встроенной поддержки области видимости методов, все методы публичные. Независимо от целей, добавление префикса `_` не сделает метод `private`-методом.

  ```jsx
  // плохо
  class Foo extends Component {
    _onClickSubmit () {
      // do stuff
    }

    // ...
  }

  // хорошо
  class Foo extends Component {
    onClickSubmit () {
      // do stuff
    }

    // ...
  }
  ```

<a name="methods--require-render-return"></a><a name="11.4"></a>
- [11.5](#methods--require-render-return) Всегда возвращайте значение из метода `render`.

  eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

  ```jsx
  // плохо
  render () {
    (<div />);
  }

  // хорошо
  render () {
    return <div />;
  }
  ```

**[К содержанию](#table-of-contents)**

## Порядок в компоненте <a name="order-in-component"></a>

<a name="order-in-component--common"></a><a name="12.1"></a>
- [12.1](#order-in-component--common) Внутри компонента организуйте код в следующем порядке:

  1. Указание типов
  2. Указание значений свойств по-умолчанию
  3. Описание свойств класса
  4. Методы жизненного цикла компонента
  5. Обработчики событий
  6. Вспомогательные методы
  7. Произвольные `render`-методы
  8. `render`

  Каждый из описанных блоков кода отбивается от предыдущего пустой строкой.

  >Почему: для легкой поддержки кода удобно группировать код по его назначению (`boilerplate`, `lifecycle`, `handlers`, `helpers`, `renders`)

<a name="order-in-component--types"></a><a name="12.2"></a>
- [12.2](#order-in-component--types) Порядок в указании типов: сначала свойства, потом состояние.

  >Почему: для единообразия с `Component<Props, State>`

  ```
  export class Example extends Component<Props, State> {
    props: Props;
    state: State;

    // ...
  }
  ```

<a name="order-in-component--defaults"></a><a name="12.3"></a>
- [12.3](#order-in-component--defaults) При указании значений свойств по умолчанию используйте алфавитный порядок; используйте ключевое слово `static`.

  >Почему: в отсортированном по алфавиту списке свойств легче ориентироваться

  ```
  static defaultProps = {
    autoFocus: false,
    id: '',
    value: defaultValue,
    required: true,
    type: 'text'
  }
  ```

<a name="order-in-component--lifecycle-methods"></a><a name="12.4"></a>
- [12.4](#order-in-component--lifecicle-methods) Порядок методов жизненного цикла следующий:

  Для `react` версии `16.2` и ниже:

  1. `componentWillMount`
  2. `componentDidMount`
  3. `componentWillReceiveProps`
  4. `shouldComponentUpdate`
  5. `componentWillUpdate`
  6. `componentDidUpdate`
  7. `componentWillUnmount`

  Для `react` версии `16.3` и выше:

  1. `getDerivedStateFromProps`
  2. `componentDidMount`
  3. `shouldComponentUpdate`
  4. `getSnapshotBeforeUpdate`
  5. `componentDidUpdate`
  6. `componentWillUnmount`

  >Почему: хронологическая сортировка

<a name="order-in-component--methods-order"></a><a name="12.5"></a>
- [12.5](#order-in-component--methods-order) Используйте сортировку по алфавиту в группах методов (обработчиков событий, вспомогательных методов, `render`-методов).

  ```
  // плохо
  renderCrumbs () {
    // ...
  }

  renderTitle () {
    // ...
  }

  renderBody () {
    // ...
  }

  render () {
    return (
      <div>
        {this.renderTitle()}
        {this.renderBody()}
      </div>
    );
  }

  // хорошо
  renderCrumbs () {
    // ...
  }

  renderBody () {
    // ...
  }

  renderTitle () {
    // ...
  }

  render () {
    return (
      <div>
        {this.renderTitle()}
        {this.renderBody()}
      </div>
    );
  }
  ```

**[К содержанию](#table-of-contents)**

## Redux <a name="redux"></a>

<a name="redux--organization"></a><a name="13.1"></a>
- [13.1](#redux--organization) Располагайте файлы действий, констант, типов и пр. рядом с файлом редьюсера.

  ```text
  src/redux-modules/
    awesomeFunctionality/
      actions.js              # действия
      awesomeFunctionality.js # редьюсер
      constants.js            # константы
      defaults.js             # предзаполненные состояния
      index.js                # экспорт редьюсера по умолчанию
      types.js                # типы
  ```

<a name="redux--use-thunk"></a><a name="13.2"></a>
- [13.2](#redux--use-thunk) Используйте `redux-thunk` и `async`/`await` для реализации асинхронных генераторов действий.

  >Почему: `async`/`await` позволяет писать более читаемый, по сравнению с цепочками `Promise`-ов код

  ```javascript
  // плохо
  const someComplexAction = (dispatch) => {
    dispatch(requestAction);

    fetch(/* ... */)
      .then(ahndleResponse)
      .then(
        json => dispatch(successAction),
        error => dispatch(errorAction)
      );
  };

  // хорошо
  const someComplexAction = async (dispatch) => {
    dispatch(requestAction);

    try {
      const json = await fetch(/* ... */);
      dispatch(successAction);
    } catch (error) {
      dispatch(errorAction);
    }
  };
  ```

<a name="redux--no-get-state-in-actions"></a><a name="13.3"></a>
- [13.3](#redux--no-get-state-in-actions) Старайтесь не использовать `getState` внутри асинхронного генератора действия.

  >Почему: в случае использования `getState` генератор действия будет связан с состоянием приложения. Если приложение решит изменить состояние (например, отработают редьюсеры) генератор действия перестанет работать.

  ```jsx
  // хрупкое решение
  const loadSomething = id => (dispatch, getState) => {
    if (!getState().items.includes(id)) {
      const something = await fetch(/* ... */);
      dispatch(addSomething(something));
    }
  }
  ```

  Можно использовать разные подходы для решения задачи:

  + перенести логику в компонент-контейнер

    ```jsx
    // хорошо
    const Container extends Component {
      handleEvent (id) {
        const {items, loadSomething} = this.props;

        if (!items.includes(id)) {
          loadSomething(id);
        }
      }
    }
    ```

  + передать необходиму часть состояния приложения как параметр

    ```jsx
    // хорошо
    const loadSomething = (id, items) => (dispatch) => {
      if (!items.includes(id) {
        const something = await fetch(/* ... */);
        dispatch(addSomething(something));
      }
    }
    ```

  + если логика принятия решения используется в нескольких местах, вынести получение участка состояния приложения в функцию, а в генератор действия передавать ключ, по которому можно получить нужную часть состояния

    ```jsx
    const getStateIn = (state, path) => _.get(state, path);

    const loadSomething = (id, path) => (dispacth, getState) => {
      if (!getStateIn(getState(), path)) {
        const something = await fetch(/* ... */);
        dispatch(addSomething(something));
      }
    }

    // ...

    loadSomething(42, 'state.awesome.items');
    ```

<a name="redux--reducers-reuse"></a><a name="13.4"></a>
- [13.4](#redux--reducers-reuse) Редьюсер нельзя переиспользовать без реализации редьюсера высшего порядка.

  ```jsx
  // не будет работать
  const poll = (state, action) => {
    switch (action.type) {
      ...
    }
  }

  const polls = combineReducers({
    closed: poll,
    open: poll
  });

  // правильное решение
  const limited = (reducer, predicate) => (state, action) => {
    if (predicate(action)) {
        return reducer(state, action);
    }

    return state;
  }

  const polls = combineReducers({
    closed: limited(poll, action => action.name === 'closed'),
    open: limited(poll, action => action.name === 'open'),
  });
  ```

<a name="redux--selectors"></a><a name="13.5"></a>
- [13.5](#redux--selectors) Используйте селекторы и помещайте их рядом с редьюсерами.

  Редьюсеры представляют срез состояния приложения, селекторы же являются языком запросов к этому срезу на уровне компонентов. Используйте селекторы для ослабления связи между моделью и представлением.

  ```jsx
  // плохо
  const mapStateToProps = state => ({
    prop1: state.awesome1.prop1,
    prop2: state.awesome2.awesome3.prop4
  });

  // хорошо
  const selectProp1 = state => state.awesome1.prop1;
  const selectProp2 = state => state.awesome2.awesome3.prop4;

  // ...

  const mapStateToProps = state => ({
    prop1: selectProp1(state),
    prop2: seelctProp2(state)
  });
  ```

  Если структура состояния приложения поменяется, необходимо будет внести изменения в селекторах, а не во всех компонентах, использующих этот участок состояния.

<a name="redux--passing-dispatch"></a><a name="13.6"></a>
- [13.6](#redux--passing-dispatch) Если `mapDispatchToProps` используется только для передачи `dispatch` в генераторы действий, отдайте предпочтение простому объекту, имена свойств которого совпадают с названиями генераторов действий.

  >Почему: код становится проще и легче читается. Привязку `dispatch` к генераторам действий выполнит функция `connect`.

  ```jsx
  // плохо
  const mapDispatchToProps = dispatch => bindActionCreators(
    {loadComments, loadPostContent},
    dispatch
  );

  /* ... */

  export default connect(null, mapDispatchToProps)(MyComponent);

  // хорошо
  const functions = {
    loadComments
    loadPostContent,
  }

  /* ... */

  export default connect(null, functions)(MyComponent)
  ```

<a name="redux--naming"></a><a name="13.7"></a>
- [13.7](#redux--naming) Придерживайтесь следующих правил именования:
    + типы действий: *[СУЩЕСТВИТЕЛЬНОЕ]_[ГЛАГОЛ]*
    + генераторы действий: *[глагол]_[Существительное]*
    + селекторы: *get[Существительное]*, *select[Существительное]*

**[К содержанию](#table-of-contents)**

## Утилиты и вспомогательные функции <a name="utils-and-helpers"></a>

<a name="utils-and-helpers--common-rules"></a><a name="14.1"></a>
- [14.1](#utils-and-helpers--common-rules) При написании утилит и вспомогательных функций придерживайтесь следующих правил:
  + группируйте функции в соответствии с их назначением;
  + используйтие именованный экспорт, не используйте экспорт по умолчанию;
  + утилиты должны быть чистыми функциями;
  + утилиты должны быть переиспользуемыми, но иметь только одну задачу.

**[К содержанию](#table-of-contents)**

## Тестирование <a name="testing"></a>

<a name="testing--data-test-id-name"></a><a name="15.1"></a>
- [15.1](#testing--data-test-id-name) Для целей тестирования используйте `data`-атрибут `testid` (т.е. `data-testid`).

  >Почему: именно с таким атрибутом работает `testing-library` и потому это название можно брать и работать с ним без дополнительных настроек.

  Можно также использовать и другие названия, например, `test-id`. При этом понадобится дополнительная настройка `testing-library`. Не очень важно само название. Важно, чтобы в рамках проекта использовалось только одно название `data`-атрибута для тестирования.

  ```jsx
  // плохо
  // Component A
  return <div data-test-id="some-value">...</div>;

  // Component B
  return <div data-testid="some-value">...</div>;

  // Component C
  return <div data-test-hook="some-value">...</div>;

  // хорошо
  // Component A
  return <div data-test-id="some-value">...</div>;

  // Component B
  return <div data-test-id="some-value">...</div>;

  // Component C
  return <div data-test-id="some-value">...</div>;
  ```

<a name="testing--data-test-id-value"></a><a name="15.2"></a>
- [15.2](#testing--data-test-id-value) Для составных значение `data`-атрибутов, предназначенных для целей тестирования используйте следующий формат значений: `ComponentName-element_modifier-or-index`, где `ComponentName` — название компонента, `element` — название одно из элементов, из которых состоит компонент. `modifier` — необязательный модификатор, указывающий, что элемент находится в каком-либо состоянии (например «включен»), `index` — необязательная уникальная строка, соответствующая элементу (например, при выводе нескольких однотипных элементов в качестве `index`-а может использоваться идентификатор элемента).

  >Почему: при отладке сразу видно, какой компонент выведен в DOM, из каких элементов он состоит в текущий момент; при написании тестов это позволяет обращаться к любому элементу из списка или к любому элементу компонента и облегчает написание вспомогательных функций для создания селекторов.

Можно использовать и любой другой стиль — `camelCase`, `PascalCase`, `snake_case` или еще что-то, но будьте последовательны: стиль написания составных значений `data`-атрибутов должен быть единообразным в рамках проекта.

  ```jsx
  // плохо
  // Component A
  return <div data-test-id="ComponentA_SubComponent_UUID">...</div>;

  // Component B
  return <div data-test-id="ComponentB-SubComponent$UUID">...</div>;

  // хорошо
  // Component A
  return <div data-test-id="ComponentA_SubComponent_UUID">...</div>;

  // Component B
  return <div data-test-id="ComponentB_SubComponent_UUID">...</div>;
  ```
**[К содержанию](#table-of-contents)**