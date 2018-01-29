# Руководство по стилю кода React

## Вопросы для проработки
- handleClick vs onClick
- handleClick = () => {} vs this.handleClick = this.handleClick.bind(this)

## Содержание <a name="table-of-contents"></a>

1. [Базовые правила](#basic-rules)
1. [Class или компонент без состояния](#classes)
1. [Именование](#naming)
1. [Выравнивание](#alignment)
1. [Кавычки](#quotes)
1. [Пробельные символы](#spacing)
1. [Свойства](#props)
1. [Ссылки]
1. [Круглые скобки]
1. [Теги]
1. [Методы]
1. [Порядок в компоненте](#order-in-component)

## Базовые правила <a name="basic-rules"></a>

<a name="basic-rules--no-multi-comp"></a><a name="1.1"></a>
- [1.1](#basic-rules--no-multi-comp) Один файл - один компонент.

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

**[К содержанию](#table-of-contents)**

## Class или компонент без состояния <a name="classes"></a>

<a name="class-vs-react-create-class"></a><a name="2.1"></a>
- [2.1](#class-vs-react-create-class) Если в компоненте есть состояние и/или ссылки, используйте `class extends React.Component`; не используйте `React.createClass`.

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

<a href="class-vs-stateless"></a><a name="2.2"></a>
- [2.2](#class-vs-stateless) Если в компоненте нет состояния и ссылок, используйте обычную функцию, а не классы `es6`.

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

**[К содержанию](#table-of-contents)**

## Именование <a name="naming"></a>

<a name="naming--extensions"></a><a name="3.1"></a>
- [3.1](#naming--extensions) Используйте расширение `.jsx` для файлов, содержащий код `React`-компонентов.

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

  >Почему: ожидается, что свойства типа `style` или `className` означают одну конкретное значение. Изменени этого `API` для своих целей делает код менее читабельным и поддерживаемым и может приводить к ошибкам.

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
- [4.1](#alignment--props) Используйте следующие пример для выравнивания свойств, закрывающей скобки и дочерних компонентов.

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

<a name="quotes--single-vs-double"></a><a name="5.1"></a>
- [5.1](#quotes--single-vs-double) Используйте двойные кавычки для значений `html`- и `JSX`-атрибутов, одинарные для строковых значений и обратные для строк-шаблонов.

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
- [7.4](#props--aria-role) Используйте только правильные, не абстрактные [роли ARIA](https://www.w3.org/TR/wai-aria/roles#role_definitions).

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

## Порядок в компоненте<a name="order-in-component"></a> (требуется обсуждение 1 из 8)

<a name="order-in-component--common"></a><a name="12.1"></a>
- [12.1](#order-in-component--common) Внутри компонента организуем код в следующем порядке:

  1. Указание типов
  2. Указание значений свойств по-умолчанию
  3. Описание свойств класса
  4. Методы жизненного цикла компонента
  5. Обработчики событий
  6. Вспомогательные методы
  7. Произвольные `render`-методы
  8. `render`

  Каждый из описанных блоков кода отбивается от предыдущего пустой строкой

  >Почему: для легкой поддержки кода решили сгруппировать код по его назначению (`boilerplate`, `lifecycle`, `handlers`, `helpers`, `renders`)

<a name="order-in-components--types"></a><a name="12.2"></a>
- [12.2](#order-in-components--types) Порядок в указании типов: сначала свойства, потом состояние

  >Почему: для единообразия с `Component<Props, State>`

  ```
  export class Example extends Component<Props, State> {
    props: Props;
    state: State;

    // ...
  }
  ```

<a name="order-in-components--defaults"></a><a name="12.3"></a>
- [12.3](#order-in-components--defaults) При указании значений свойств по-умолчанию используем алфавитный порядок; используем ключевое слово `static`

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

  >Не обосновано: использование `static`

<a name="order-in-components--lifecycle-methods"></a><a name="12.4"></a>
- [12.4](#order-in-components--lifecicle-methods) Порядок методов жизненного цикла следующий:

  1. `componentWillMount`
  2. `componentDidMount`
  3. `componentWillReceiveProps`
  4. `shouldComponentUpdate`
  5. `componentWillUpdate`
  6. `componentDidUpdate`
  7. `componentWillUnmount`

  >Почему: хронологическая сортировка

<a name="order-in-components--methods-order"></a><a name="12.5"></a>
- [12.5](#order-in-components--methods-order) Сортировка методов (обработчиков событий, вспомогательных методов, `render`-методов) должна соответствовать порядку их использования в коде; не стоит использовать сортировку по алфавиту

  >Почему: порядок использования дает примерное понимание, где в коде находится метод; алфавитная сортировка предполагает перемещение в коде метода в случае переименования, что затрудняет понимание `diff`-ов и приводит в сложностям при слиянии веток

  ```
  // плохо
  render Crumbs () {
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

  // хорошо
  render Crumbs () {
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
  ```

**[К содержанию](#table-of-contents)**
