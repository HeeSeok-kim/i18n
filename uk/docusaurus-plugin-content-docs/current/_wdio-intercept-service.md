---
id: wdio-intercept-service
title: Служба перехоплення
custom_edit_url: https://github.com/webdriverio-community/wdio-intercept-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-intercept-service це сторонній пакет, для отримання додаткової інформації, будь ласка, відвідайте [GitHub](https://github.com/webdriverio-community/wdio-intercept-service) | [npm](https://www.npmjs.com/package/wdio-intercept-service)

🕸 Перехоплюйте та перевіряйте HTTP ajax-запити в [webdriver.io](http://webdriver.io/)

[![Tests](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml/badge.svg)](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml) [![Join the chat on Discord](https://img.shields.io/discord/1097401827202445382?logo=discord&logoColor=FFFFFF&color=5865F2)](https://discord.webdriver.io/)

Це плагін для [webdriver.io](http://webdriver.io/). Якщо ви ще не знайомі з ним, перевірте його, він досить крутий.

Хоча selenium і webdriver використовуються для e2e і особливо для UI-тестування, ви можете захотіти оцінити HTTP-запити, зроблені вашим клієнтським кодом (наприклад, коли у вас немає безпосереднього відгуку UI, як у метриках або відстеженні викликів). З wdio-intercept-service ви можете перехоплювати ajax HTTP-виклики, ініційовані деякою дією користувача (наприклад, натисканням кнопки тощо) і робити твердження про запит і відповідні відповіді пізніше.

Але є одне застереження: ви не можете перехоплювати HTTP-виклики, які ініціюються при завантаженні сторінки (як у більшості SPA), оскільки це вимагає деяких налаштувань, які можуть бути виконані тільки після завантаження сторінки (через обмеження в selenium). **Це означає, що ви можете просто захоплювати запити, які були ініційовані всередині тесту.** Якщо вас це влаштовує, цей плагін може бути для вас, тому читайте далі.

## Передумови

* webdriver.io **v5.x** або новіший.

**Увага! Якщо ви все ще використовуєте webdriver.io v4, будь ласка, використовуйте гілку v2.x цього плагіна!**

## Встановлення

```shell
npm install wdio-intercept-service -D
```

## Використання

### Використання з WebDriver CLI

Це повинно бути так само просто, як додавання wdio-intercept-service до вашого `wdio.conf.js`:

```javascript
exports.config = {
  // ...
  services: ['intercept']
  // ...
};
```

і все готово.

### Використання з WebDriver Standalone

При використанні WebdriverIO Standalone, функції `before` та `beforeTest` / `beforeScenario` потрібно викликати вручну.

```javascript
import { remote } from 'webdriverio';
import WebdriverAjax from 'wdio-intercept-service'

const WDIO_OPTIONS = {
  port: 9515,
  path: '/',
  capabilities: {
    browserName: 'chrome'
  },
}

let browser;
const interceptServiceLauncher = WebdriverAjax();

beforeAll(async () => {
  browser = await remote(WDIO_OPTIONS)
  interceptServiceLauncher.before(null, null, browser)
})

beforeEach(async () => {
  interceptServiceLauncher.beforeTest()
})

afterAll(async () => {
  await client.deleteSession()
});

describe('', async () => {
  ... // See example usage
});
```

Після ініціалізації, деякі пов'язані функції додаються до ланцюга команд вашого браузера (див. [API](#api)).

## Швидкий старт

Приклад використання:

```javascript
browser.url('http://foo.bar');
browser.setupInterceptor(); // capture ajax calls
browser.expectRequest('GET', '/api/foo', 200); // expect GET request to /api/foo with 200 statusCode
browser.expectRequest('POST', '/api/foo', 400); // expect POST request to /api/foo with 400 statusCode
browser.expectRequest('GET', /\/api\/foo/, 200); // can validate a URL with regex, too
browser.click('#button'); // button that initiates ajax request
browser.pause(1000); // maybe wait a bit until request is finished
browser.assertRequests(); // validate the requests
```

Отримати деталі про запити:

```javascript
browser.url('http://foo.bar')
browser.setupInterceptor();
browser.click('#button')
browser.pause(1000);

var request = browser.getRequest(0);
assert.equal(request.method, 'GET');
assert.equal(request.response.headers['content-length'], '42');
```

## Підтримувані браузери

Повинно працювати з досить новими версіями всіх браузерів. Будь ласка, повідомте про проблему, якщо щось не працює з вашим браузером.

## API

Зверніться до файлу декларації TypeScript для повного синтаксису користувацьких команд, доданих до об'єкта браузера WebdriverIO. Загалом, будь-який метод, який приймає об'єкт "options" як параметр, може бути викликаний без цього параметра для отримання поведінки за замовчуванням. Ці "необов'язкові" об'єкти options позначені `?: = {}` і їхні значення за замовчуванням описані для кожного методу.

### Опис опцій

Ця бібліотека пропонує невелику кількість конфігурацій при виклику команд. Тут описані параметри конфігурації, які використовуються багатьма методами (дивіться визначення кожного методу, щоб визначити конкретну підтримку).

* `orderBy` (`'START' | 'END'`): Ця опція контролює порядок запитів, перехоплених перехоплювачем, коли вони повертаються до вашого тесту. Для зворотної сумісності з існуючими версіями цієї бібліотеки, порядок за замовчуванням - `'END'`, що відповідає моменту завершення запиту. Якщо ви встановите опцію `orderBy` на `'START'`, тоді запити будуть впорядковані відповідно до часу їх початку.
* `includePending` (`boolean`): Ця опція контролює, чи будуть повертатися ще не завершені запити. Для зворотної сумісності з існуючими версіями цієї бібліотеки, значення за замовчуванням `false`, і будуть повертатися тільки завершені запити.

### browser.setupInterceptor()

Перехоплює ajax-виклики в браузері. Ви завжди повинні викликати функцію налаштування, щоб пізніше оцінити запити.

### browser.disableInterceptor()

Запобігає подальшому перехопленню ajax-викликів у браузері. Вся інформація про перехоплені запити видаляється. Більшість користувачів не потребують відключення перехоплювача, але якщо тест працює особливо довго або перевищує ємність сесійного сховища, то відключення перехоплювача може бути корисним.

### `browser.excludeUrls(urlRegexes: (string | RegExp)[])`

Виключає запити з певних URL-адрес з записування. Він приймає масив рядків або регулярних виразів. Перед записом у сховище,
перевіряє URL запиту на кожен рядок або регулярний вираз. Якщо він відповідає, запит не записується в сховище. Як і disableInterceptor, це може бути корисно 
якщо виникають проблеми з перевищенням ємності сесійного сховища.

### browser.expectRequest(method: string, url: string, statusCode: number)

Створює очікування щодо ajax-запитів, які будуть ініційовані під час тесту. Може (і повинно) бути ланцюговим. Порядок очікувань повинен відповідати порядку запитів, що виконуються.

* `method` (`String`): HTTP-метод, який очікується. Може бути будь-яким, що `xhr.open()` приймає як перший аргумент.
* `url` (`String`|`RegExp`): точний URL, який викликається в запиті, як рядок або RegExp для збігу
* `statusCode` (`Number`): очікуваний код статусу відповіді

### browser.getExpectations()

Допоміжний метод. Повертає всі очікування, які ви створили до цього моменту

### browser.resetExpectations()

Допоміжний метод. Скидає всі очікування, які ви створили до цього моменту

### `browser.assertRequests({ orderBy?: 'START' | 'END' }?: = {})`

Викличте цей метод, коли всі очікувані ajax-запити завершені. Він порівнює очікування з фактичними зробленими запитами і перевіряє наступне:

- Кількість запитів, які були зроблені
- Порядок запитів
- Метод, URL та statusCode повинні збігатися для кожного зробленого запиту
- Об'єкт options за замовчуванням має значення `{ orderBy: 'END' }`, тобто за часом завершення запитів, щоб бути узгодженим з поведінкою v4.1.10 і раніше. Коли опція `orderBy` встановлена на `'START'`, запити будуть упорядковані за часом їх ініціювання сторінкою.

### `browser.assertExpectedRequestsOnly({ inOrder?: boolean, orderBy?: 'START' | 'END' }?: = {})`

Подібно до `browser.assertRequests`, але перевіряє тільки запити, які ви вказали у своїх директивах `expectRequest`, без необхідності відображати всі мережеві запити, які можуть відбуватися навколо цього. Якщо опція `inOrder` встановлена в `true` (за замовчуванням), очікується, що запити будуть знайдені в тому ж порядку, в якому вони були налаштовані за допомогою `expectRequest`.

### `browser.getRequest(index: number, { includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

Для більш складних тверджень про конкретний запит ви можете отримати деталі для певного запиту. Ви повинні надати 0-based індекс запиту, до якого ви хочете отримати доступ, в порядку, в якому запити були завершені (за замовчуванням), або ініційовані (передавши опцію `orderBy: 'START'`).

* `index` (`number`): номер запиту, до якого ви хочете отримати доступ
* `options` (`object`): Опції конфігурації
* `options.includePending` (`boolean`): Чи повинні повертатися ще не завершені запити. За замовчуванням це false, щоб відповідати поведінці бібліотеки у версії v4.1.10 і раніше.
* `options.orderBy` (`'START' | 'END'`): Як повинні бути упорядковані запити. За замовчуванням це `'END'`, щоб відповідати поведінці бібліотеки у версії v4.1.10 і раніше. Якщо `'START'`, запити будуть упорядковані за часом ініціювання, а не за часом завершення запиту. (Оскільки очікуваний запит ще не завершений, при упорядкуванні за `'END'` всі очікувані запити будуть йти після всіх завершених запитів.)

**Повертає** об'єкт `request`:

* `request.url`: запитуваний URL
* `request.method`: використаний HTTP-метод
* `request.body`: дані корисного навантаження/тіла, використані в запиті
* `request.headers`: HTTP-заголовки запиту як JS-об'єкт
* `request.pending`: булевий прапорець, що вказує, чи цей запит завершений (тобто має властивість `response`), або в процесі.
* `request.response`: JS-об'єкт, який присутній тільки якщо запит завершено (тобто `request.pending === false`), що містить дані про відповідь.
* `request.response?.headers`: HTTP-заголовки відповіді як JS-об'єкт
* `request.response?.body`: тіло відповіді (буде розпарсено як JSON, якщо можливо)
* `request.response?.statusCode`: код статусу відповіді

**Примітка щодо `request.body`:** wdio-intercept-service спробує розпарсити тіло запиту наступним чином:

* string: Просто повертає рядок (`'value'`)
* JSON: Парсить JSON-об'єкт за допомогою `JSON.parse()` (`({ key: value })`)
* FormData: Виведе FormData у форматі `{ key: [value1, value2, ...] }`
* ArrayBuffer: Спробує перетворити буфер на рядок (експериментально)
* Все інше: Буде використовувати грубий `JSON.stringify()` на ваших даних. Удачі!

**Для API `fetch` ми підтримуємо тільки рядкові та JSON дані!**

### `browser.getRequests({ includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

Отримати всі перехоплені запити як масив, з підтримкою тих же необов'язкових опцій, що і `getRequest`.

**Повертає** масив об'єктів `request`.

### browser.hasPendingRequests()

Службовий метод, який перевіряє, чи є ще HTTP-запити в очікуванні. Може використовуватися тестами для забезпечення того, щоб всі запити завершилися протягом розумного проміжку часу, або для перевірки того, що виклик `getRequests()` або `assertRequests()` включатиме всі бажані HTTP-запити.

**Повертає** булеве значення

## Підтримка TypeScript

Цей плагін надає власні типи TS. Просто вкажіть свій tsconfig на розширення типів, як зазначено [тут](https://webdriver.io/docs/typescript.html#framework-types):

```
"compilerOptions": {
    // ..
    "types": ["node", "webdriverio", "wdio-intercept-service"]
},
```

## Запуск тестів

Для локального запуску тестів потрібні останні версії Chrome та Firefox. Можливо, вам доведеться оновити залежності `chromedriver` та `geckodriver` відповідно до версії, встановленої у вашій системі.

```shell
npm test
```

## Внесення змін

Я радий кожному внеску. Просто відкрийте issue або безпосередньо надішліть PR.  
Зверніть увагу, що ця бібліотека перехоплювача написана для роботи з legacy-браузерами, такими як Internet Explorer. Таким чином, будь-який код, використаний у `lib/interceptor.js`, повинен бути принаймні розпізнаваним для середовища виконання JavaScript Internet Explorer.

## Ліцензія

MIT