---
id: selectors
title: Селекторы
---

[Протокол WebDriver](https://w3c.github.io/webdriver/) предоставляет несколько стратегий селекторов для запроса элементов. WebdriverIO упрощает их, чтобы выбор элементов оставался простым. Обратите внимание, что хотя команды для запроса элементов называются `$` и `$$`, они не имеют ничего общего с jQuery или [движком селекторов Sizzle](https://github.com/jquery/sizzle).

Хотя доступно так много различных селекторов, только некоторые из них обеспечивают надежный способ найти правильный элемент. Например, для следующей кнопки:

```html
<button
  id="main"
  class="btn btn-large"
  name="submission"
  role="button"
  data-testid="submit"
>
  Submit
</button>
```

Мы __рекомендуем__ и __не рекомендуем__ следующие селекторы:

| Селектор | Рекомендуется | Примечания |
| -------- | ----------- | ----- |
| `$('button')` | 🚨 Никогда | Худший - слишком общий, нет контекста. |
| `$('.btn.btn-large')` | 🚨 Никогда | Плохо. Связан со стилями. Очень подвержен изменениям. |
| `$('#main')` | ⚠️ Редко | Лучше. Но все еще связан со стилями или обработчиками событий JS. |
| `$(() => document.queryElement('button'))` | ⚠️ Редко | Эффективный запрос, сложно писать. |
| `$('button[name="submission"]')` | ⚠️ Редко | Связан с атрибутом `name`, который имеет HTML-семантику. |
| `$('button[data-testid="submit"]')` | ✅ Хорошо | Требует дополнительного атрибута, не связанного с доступностью. |
| `$('aria/Submit')` или `$('button=Submit')` | ✅ Всегда | Лучший вариант. Отражает то, как пользователь взаимодействует со страницей. Рекомендуется использовать файлы переводов вашего фронтенда, чтобы ваши тесты никогда не падали при обновлении переводов |

## CSS-селектор запроса

Если не указано иное, WebdriverIO будет запрашивать элементы, используя шаблон [CSS-селектора](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors), например:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L7-L8
```

## Текст ссылки

Чтобы получить элемент якоря с определенным текстом в нем, запросите текст, начинающийся со знака равенства (`=`).

Например:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L3
```

Вы можете запросить этот элемент, вызвав:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L16-L18
```

## Частичный текст ссылки

Чтобы найти элемент якоря, видимый текст которого частично соответствует вашему поисковому значению,
запросите его, используя `*=` перед строкой запроса (например, `*=driver`).

Вы можете запросить элемент из примера выше, также вызвав:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L24-L26
```

__Примечание:__ Вы не можете смешивать несколько стратегий селекторов в одном селекторе. Используйте несколько цепочек запросов элементов для достижения той же цели, например:

```js
const elem = await $('header h1*=Welcome') // не работает!!!
// используйте вместо этого
const elem = await $('header').$('*=driver')
```

## Элемент с определенным текстом

Та же техника может быть применена и к элементам. Кроме того, также возможно выполнять сопоставление без учета регистра, используя `.=` или `.*=` в запросе.

Например, вот запрос для заголовка уровня 1 с текстом "Welcome to my Page":

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L2
```

Вы можете запросить этот элемент, вызвав:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L35C1-L38
```

Или используя запрос частичного текста:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L44C9-L47
```

То же самое работает для имен `id` и `class`:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L4
```

Вы можете запросить этот элемент, вызвав:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L49-L67
```

__Примечание:__ Вы не можете смешивать несколько стратегий селекторов в одном селекторе. Используйте несколько цепочек запросов элементов для достижения той же цели, например:

```js
const elem = await $('header h1*=Welcome') // не работает!!!
// используйте вместо этого
const elem = await $('header').$('h1*=Welcome')
```

## Имя тега

Чтобы запросить элемент с определенным именем тега, используйте `<tag>` или `<tag />`.

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L5
```

Вы можете запросить этот элемент, вызвав:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L61-L62
```

## Атрибут Name

Для запроса элементов с определенным атрибутом name можно использовать либо обычный селектор CSS3, либо предоставленную стратегию имени из [JSONWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol), передав что-то вроде [name="some-name"] в качестве параметра селектора:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L68-L69
```

__Примечание:__ Эта стратегия селектора устарела и работает только в старых браузерах, которые работают по протоколу JSONWireProtocol, или при использовании Appium.

## xPath

Также возможно запрашивать элементы через определенный [xPath](https://developer.mozilla.org/en-US/docs/Web/XPath).

Селектор xPath имеет формат типа `//body/div[6]/div[1]/span[1]`.

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/xpath.html
```

Вы можете запросить второй параграф, вызвав:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L75-L76
```

Вы можете использовать xPath также для перемещения вверх и вниз по дереву DOM:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L78-L79
```

## Селектор доступного имени

Запрашивайте элементы по их доступному имени. Доступное имя — это то, что объявляет программа чтения с экрана, когда этот элемент получает фокус. Значение доступного имени может быть как визуальным содержимым, так и скрытыми текстовыми альтернативами.

:::info

Вы можете узнать больше об этом селекторе в нашем [блоге о выпуске](/blog/2022/09/05/accessibility-selector)

:::

### Извлечение по `aria-label`

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L1
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L86-L87
```

### Извлечение по `aria-labelledby`

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L2-L3
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L93-L94
```

### Извлечение по содержимому

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L4
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L100-L101
```

### Извлечение по заголовку

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L5
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L107-L108
```

### Извлечение по свойству `alt`

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L114-L115
```

## ARIA - Атрибут Role

Для запроса элементов на основе [ролей ARIA](https://www.w3.org/TR/html-aria/#docconformance) вы можете напрямую указать роль элемента, например `[role=button]` в качестве параметра селектора:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L13
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L131-L132
```

## Атрибут ID

Стратегия локатора "id" не поддерживается в протоколе WebDriver, вместо этого следует использовать стратегии селекторов CSS или xPath для поиска элементов по ID.

Однако некоторые драйверы (например, [Appium You.i Engine Driver](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies)) могут все еще [поддерживать](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies) этот селектор.

Текущие поддерживаемые синтаксисы селекторов для ID:

```js
//css локатор
const button = await $('#someid')
//xpath локатор
const button = await $('//*[@id="someid"]')
//стратегия id
// Примечание: работает только в Appium или подобных фреймворках, которые поддерживают стратегию локатора "ID"
const button = await $('id=resource-id/iosname')
```

## JS-функция

Вы также можете использовать JavaScript-функции для получения элементов, используя нативные веб-API. Конечно, вы можете делать это только внутри веб-контекста (например, `browser` или веб-контекст в мобильных устройствах).

Учитывая следующую HTML-структуру:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/js.html
```

Вы можете запросить соседний элемент `#elem` следующим образом:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L139-L143
```

## Глубокие селекторы

:::warning

Начиная с `v9` WebdriverIO, нет необходимости в этом специальном селекторе, так как WebdriverIO автоматически проникает через Shadow DOM за вас. Рекомендуется перейти с этого селектора, удалив `>>>` перед ним.

:::

Многие фронтенд-приложения сильно полагаются на элементы с [теневым DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM). Технически невозможно запрашивать элементы внутри теневого DOM без обходных путей. [`shadow$`](https://webdriver.io/docs/api/element/shadow$) и [`shadow$$`](https://webdriver.io/docs/api/element/shadow$$) были такими обходными путями, которые имели свои [ограничения](https://github.com/Georgegriff/query-selector-shadow-dom#how-is-this-different-to-shadow). С глубоким селектором вы теперь можете запрашивать все элементы внутри любого теневого DOM, используя общую команду запроса.

Предположим, у нас есть приложение со следующей структурой:

![Chrome Example](https://github.com/Georgegriff/query-selector-shadow-dom/raw/main/Chrome-example.png "Chrome Example")

С этим селектором вы можете запросить элемент `<button />`, который вложен в другой теневой DOM, например:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L147-L149
```

## Мобильные селекторы

Для гибридного мобильного тестирования важно, чтобы сервер автоматизации находился в правильном *контексте* перед выполнением команд. Для автоматизации жестов драйвер в идеале должен быть установлен в нативный контекст. Но для выбора элементов из DOM драйвер должен быть установлен в контекст webview платформы. Только *после этого* можно использовать упомянутые выше методы.

Для нативного мобильного тестирования нет переключения между контекстами, так как вы должны использовать мобильные стратегии и напрямую использовать базовую технологию автоматизации устройства. Это особенно полезно, когда тест требует тонкого контроля над поиском элементов.

### Android UiAutomator

Фреймворк UI Automator для Android предоставляет несколько способов поиска элементов. Вы можете использовать [API UI Automator](https://developer.android.com/tools/testing-support-library/index.html#uia-apis), в частности, [класс UiSelector](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) для локализации элементов. В Appium вы отправляете Java-код в виде строки на сервер, который выполняет его в среде приложения, возвращая элемент или элементы.

```js
const selector = 'new UiSelector().text("Cancel").className("android.widget.Button")'
const button = await $(`android=${selector}`)
await button.click()
```

### Android DataMatcher и ViewMatcher (только Espresso)

Стратегия DataMatcher Android предоставляет способ поиска элементов по [Data Matcher](https://developer.android.com/reference/android/support/test/espresso/DataInteraction)

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"]
})
await menuItem.click()
```

И аналогично [View Matcher](https://developer.android.com/reference/android/support/test/espresso/ViewInteraction)

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"],
  "class": "androidx.test.espresso.matcher.ViewMatchers"
})
await menuItem.click()
```

### Android View Tag (только Espresso)

Стратегия тегов представления предоставляет удобный способ поиска элементов по их [тегам](https://developer.android.com/reference/android/support/test/espresso/matcher/ViewMatchers.html#withTagValue%28org.hamcrest.Matcher%3Cjava.lang.Object%3E%29).

```js
const elem = await $('-android viewtag:tag_identifier')
await elem.click()
```

### iOS UIAutomation

При автоматизации приложения iOS можно использовать [фреймворк UI Automation](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) от Apple для поиска элементов.

Этот JavaScript [API](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/index.html#//apple_ref/doc/uid/TP40009771) имеет методы для доступа к представлению и всему, что на нем находится.

```js
const selector = 'UIATarget.localTarget().frontMostApp().mainWindow().buttons()[0]'
const button = await $(`ios=${selector}`)
await button.click()
```

Вы также можете использовать поиск предикатов в iOS UI Automation в Appium для дальнейшего уточнения выбора элементов. Подробности см. [здесь](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/ios/ios-predicate.md).

### iOS XCUITest строки предикатов и цепочки классов

С iOS 10 и выше (при использовании драйвера `XCUITest`) вы можете использовать [строки предикатов](https://github.com/facebook/WebDriverAgent/wiki/Predicate-Queries-Construction-Rules):

```js
const selector = `type == 'XCUIElementTypeSwitch' && name CONTAINS 'Allow'`
const switch = await $(`-ios predicate string:${selector}`)
await switch.click()
```

И [цепочки классов](https://github.com/facebook/WebDriverAgent/wiki/Class-Chain-Queries-Construction-Rules):

```js
const selector = '**/XCUIElementTypeCell[`name BEGINSWITH "D"`]/**/XCUIElementTypeButton'
const button = await $(`-ios class chain:${selector}`)
await button.click()
```

### Accessibility ID

Стратегия локатора `accessibility id` предназначена для чтения уникального идентификатора элемента пользовательского интерфейса. Это имеет преимущество в том, что не меняется при локализации или любом другом процессе, который может изменить текст. Кроме того, это может помочь в создании кросс-платформенных тестов, если элементы, которые функционально одинаковы, имеют одинаковый accessibility id.

- Для iOS это `accessibility identifier`, установленный Apple [здесь](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAccessibilityIdentification_Protocol/index.html).
- Для Android `accessibility id` соответствует `content-description` для элемента, как описано [здесь](https://developer.android.com/training/accessibility/accessible-app.html).

Для обеих платформ получение элемента (или нескольких элементов) по их `accessibility id` обычно является лучшим методом. Это также предпочтительный способ по сравнению с устаревшей стратегией `name`.

```js
const elem = await $('~my_accessibility_identifier')
await elem.click()
```

### Class Name

Стратегия `class name` - это `строка`, представляющая элемент пользовательского интерфейса в текущем представлении.

- Для iOS это полное имя [класса UIAutomation](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html), и будет начинаться с `UIA-`, например, `UIATextField` для текстового поля. Полную справку можно найти [здесь](https://developer.apple.com/library/ios/navigation/#section=Frameworks&topic=UIAutomation).
- Для Android это полностью квалифицированное имя [класса UI Automator](https://developer.android.com/tools/testing-support-library/index.html#UIAutomator) [class](https://developer.android.com/reference/android/widget/package-summary.html), например, `android.widget.EditText` для текстового поля. Полную справку можно найти [здесь](https://developer.android.com/reference/android/widget/package-summary.html).
- Для Youi.tv это полное имя класса Youi.tv, и будет начинаться с `CYI-`, например, `CYIPushButtonView` для элемента кнопки. Полную справку можно найти на [странице GitHub драйвера You.i Engine](https://github.com/YOU-i-Labs/appium-youiengine-driver)

```js
// пример iOS
await $('UIATextField').click()
// пример Android
await $('android.widget.DatePicker').click()
// пример Youi.tv
await $('CYIPushButtonView').click()
```

## Цепочки селекторов

Если вы хотите быть более конкретным в своем запросе, вы можете связать селекторы в цепочку, пока не найдете нужный элемент. Если вы вызываете `element` перед вашей фактической командой, WebdriverIO начнет запрос с этого элемента.

Например, если у вас есть структура DOM:

```html
<div class="row">
  <div class="entry">
    <label>Product A</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
  <div class="entry">
    <label>Product B</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
  <div class="entry">
    <label>Product C</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
</div>
```

И вы хотите добавить продукт B в корзину, было бы сложно сделать это только с помощью CSS-селектора.

С цепочкой селекторов это гораздо проще. Просто сужайте нужный элемент шаг за шагом:

```js
await $('.row .entry:nth-child(2)').$('button*=Add').click()
```

### Селектор изображения Appium

Используя стратегию локатора `-image`, можно отправить в Appium файл изображения, представляющий элемент, к которому вы хотите получить доступ.

Поддерживаемые форматы файлов `jpg,png,gif,bmp,svg`

Полную справку можно найти [здесь](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md)

```js
const elem = await $('./file/path/of/image/test.jpg')
await elem.click()
```

**Примечание**: Способ работы Appium с этим селектором заключается в том, что он внутренне сделает (app)скриншот и использует предоставленный селектор изображения
для проверки, может ли элемент быть найден на этом (app)скриншоте.

Учтите, что Appium может изменить размер сделанного (app)скриншота, чтобы он соответствовал CSS-размеру вашего (app)экрана (это произойдет
на iPhone, а также на Mac-компьютерах с дисплеем Retina, потому что DPR больше 1). Это приведет к тому, что совпадение не будет найдено, потому что предоставленный селектор изображения мог быть взят из оригинального скриншота.
Вы можете исправить это, обновив настройки сервера Appium, см. [документацию Appium](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md#related-settings) для настроек и [этот комментарий](https://github.com/webdriverio/webdriverio/issues/6097#issuecomment-726675579) с подробным объяснением.

## Селекторы React

WebdriverIO предоставляет способ выбора компонентов React на основе имени компонента. Для этого у вас есть выбор из двух команд: `react$` и `react$$`.

Эти команды позволяют выбирать компоненты из [React VirtualDOM](https://reactjs.org/docs/faq-internals.html) и возвращать либо один элемент WebdriverIO, либо массив элементов (в зависимости от того, какая функция используется).

**Примечание**: Команды `react$` и `react$$` аналогичны по функциональности, за исключением того, что `react$$` возвращает *все* соответствующие экземпляры в виде массива элементов WebdriverIO, а `react$` возвращает первый найденный экземпляр.

#### Базовый пример

```jsx
// index.jsx
import React from 'react'
import ReactDOM from 'react-dom'

function MyComponent() {
    return (
        <div>
            MyComponent
        </div>
    )
}

function App() {
    return (<MyComponent />)
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

В приведенном выше коде есть простой экземпляр `MyComponent` внутри приложения, который React рендерит внутри HTML-элемента с `id="root"`.

С помощью команды `browser.react$` вы можете выбрать экземпляр `MyComponent`:

```js
const myCmp = await browser.react$('MyComponent')
```

Теперь, когда у вас есть элемент WebdriverIO, хранящийся в переменной `myCmp`, вы можете выполнять команды элемента с ним.

#### Фильтрация компонентов

Библиотека, которую WebdriverIO использует внутренне, позволяет фильтровать ваш выбор по свойствам и/или состоянию компонента. Для этого вам нужно передать второй аргумент для свойств и/или третий аргумент для состояния в команду браузера.

```jsx
// index.jsx
import React from 'react'
import ReactDOM from 'react-dom'

function MyComponent(props) {
    return (
        <div>
            Hello { props.name || 'World' }!
        </div>
    )
}

function App() {
    return (
        <div>
            <MyComponent name="WebdriverIO" />
            <MyComponent />
        </div>
    )
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

Если вы хотите выбрать экземпляр `MyComponent`, у которого есть свойство `name` со значением `WebdriverIO`, вы можете выполнить команду следующим образом:

```js
const myCmp = await browser.react$('MyComponent', {
    props: { name: 'WebdriverIO' }
})
```

Если вы хотели бы фильтровать наш выбор по состоянию, команда `browser` будет выглядеть примерно так:

```js
const myCmp = await browser.react$('MyComponent', {
    state: { myState: 'some value' }
})
```

#### Работа с `React.Fragment`

При использовании команды `react$` для выбора [фрагментов](https://reactjs.org/docs/fragments.html) React, WebdriverIO вернет первый дочерний элемент этого компонента в качестве узла компонента. Если вы используете `react$$`, вы получите массив, содержащий все HTML-узлы внутри фрагментов, которые соответствуют селектору.

```jsx
// index.jsx
import React from 'react'
import ReactDOM from 'react-dom'

function MyComponent() {
    return (
        <React.Fragment>
            <div>
                MyComponent
            </div>
            <div>
                MyComponent
            </div>
        </React.Fragment>
    )
}

function App() {
    return (<MyComponent />)
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

Учитывая приведенный выше пример, вот как будут работать команды:

```js
await browser.react$('MyComponent') // возвращает элемент WebdriverIO для первого <div />
await browser.react$$('MyComponent') // возвращает элементы WebdriverIO для массива [<div />, <div />]
```

**Примечание:** Если у вас несколько экземпляров `MyComponent` и вы используете `react$$` для выбора этих компонентов-фрагментов, вам будет возвращен одномерный массив всех узлов. Другими словами, если у вас есть 3 экземпляра `<MyComponent />`, вам будет возвращен массив с шестью элементами WebdriverIO.

## Пользовательские стратегии селекторов


Если ваше приложение требует особого способа получения элементов, вы можете самостоятельно определить пользовательскую стратегию селекторов, которую можно использовать с `custom$` и `custom$$`. Для этого зарегистрируйте свою стратегию один раз в начале теста, например, в хуке `before`:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L2-L11
```

Учитывая следующий HTML-фрагмент:

```html reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/example.html#L8-L12
```

Затем используйте его, вызвав:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L16-L19
```

**Примечание:** это работает только в веб-окружении, в котором может быть запущена команда [`execute`](/docs/api/browser/execute).