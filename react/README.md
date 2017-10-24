# Руководство по стилю кода React

## Вопросы для проработки

## Содержание <a name="table-of-contents"></a>

- [Базовые правила](#basic-rules)
- [Class или компонент без состояния]
- [Именование]
- [Выравнивание]
- [Кавычки]
- [Пробельные символы]
- [Свойства]
- [Ссылки]
- [Круглые скобки]
- [Теги]
- [Методы]
- [Порядок в компоненте](#order-in-component)

## Базовые правила <a name="basic-rules"></a>

<a name="basic-rules--no-multi-comp"></a><a name="1.1"></a>
- [1.1](#basic-rules--no-multi-comp) Один файл - один компонент

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

## Порядок в компоненте<a name="order-in-component"></a> (требуется обсуждение 1 из 8)

<a name="order-in-component--common"></a><a name="x.1"></a>
- [x.1](#order-in-component--common) Внутри компонента организуем код в следующем порядке:

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

<a name="order-in-components--types"></a><a name="x.2"></a>
- [x.2](#order-in-components--types) Порядок в указании типов: сначала свойства, потом состояние

  >Почему: для единообразия с `Component<Props, State>`

  ```
  export class Example extends Component<Props, State> {
    props: Props;
    state: State;

    // ...
  }
  ```

<a name="order-in-components--defaults"></a><a name="x.3"></a>
- [x.3](#order-in-components--defaults) При указании значений свойств по-умолчанию используем алфавитный порядок; используем ключевое слово `static`

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

<a name="order-in-components--lifecycle-methods"></a><a name="x.4"></a>
- [x.4](#order-in-components--lifecicle-methods) Порядок методов жизненного цикла следующий:

  1. `componentWillMount`
  2. `componentDidMount`
  3. `componentWillReceiveProps`
  4. `shouldComponentUpdate`
  5. `componentWillUpdate`
  6. `componentDidUpdate`
  7. `componentWillUnmount`

  >Почему: хронологическая сортировка

<a name="order-in-components--methods-order"></a><a name="x.5"></a>
- [x.5](#order-in-components--methods-order) Сортировка методов (обработчиков событий, вспомогательных методов, `render`-методов) должна соответствовать порядку их использования в коде; не стоит использовать сортировку по алфавиту

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