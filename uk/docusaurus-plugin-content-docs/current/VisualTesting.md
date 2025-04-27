---
id: visual-testing
title: Візуальне тестування
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Що він може робити?

WebdriverIO забезпечує порівняння зображень на екранах, елементах або повної сторінки для

-   🖥️ Десктопних браузерів (Chrome / Firefox / Safari / Microsoft Edge)
-   📱 Мобільних / Планшетних браузерів (Chrome на Android емуляторах / Safari на iOS Симуляторах / Симуляторах / реальних пристроях) через Appium
-   📱 Нативних додатків (Android емулятори / iOS Симулятори / реальні пристрої) через Appium (🌟 **НОВИНКА** 🌟)
-   📳 Гібридних додатків через Appium

через [`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service), який є легким сервісом WebdriverIO.

Це дозволяє вам:

-   зберігати або порівнювати **екрани/елементи/повносторінкові** знімки з базовим зображенням
-   автоматично **створювати базове зображення**, коли його немає
-   **блокувати власні області** і навіть **автоматично виключати** рядок стану та/або панелі інструментів (лише для мобільних) під час порівняння
-   збільшувати розміри знімків елементів
-   **приховувати текст** під час порівняння веб-сайтів для:
    -   **покращення стабільності** та запобігання нестабільності відтворення шрифтів
    -   зосередження лише на **макеті** веб-сайту
-   використовувати **різні методи порівняння** та набір **додаткових зіставників** для більш читабельних тестів
-   перевіряти, як ваш веб-сайт **підтримуватиме переміщення клавішею табуляції**, дивіться також [Переміщення по сайту за допомогою клавіші Tab](#tabbing-through-a-website)
-   і багато іншого, дивіться опції [сервісу](./visual-testing/service-options) та [методів](./visual-testing/method-options)

Сервіс є легким модулем для отримання необхідних даних та знімків екрана для всіх браузерів/пристроїв. Потужність порівняння забезпечується [ResembleJS](https://github.com/Huddle/Resemble.js). Якщо ви хочете порівнювати зображення онлайн, ви можете перевірити [онлайн-інструмент](http://rsmbl.github.io/Resemble.js/).

:::info ПРИМІТКА для нативних/гібридних додатків
Методи `saveScreen`, `saveElement`, `checkScreen`, `checkElement` та зіставники `toMatchScreenSnapshot` і `toMatchElementSnapshot` можуть використовуватися для нативних додатків/контексту.

Будь ласка, використовуйте властивість `isHybridApp:true` у налаштуваннях сервісу, якщо ви хочете використовувати його для гібридних додатків.
:::

## Встановлення

Найпростіший спосіб — зберегти `@wdio/visual-service` як dev-залежність у вашому `package.json`, через:

```sh
npm install --save-dev @wdio/visual-service
```

## Використання

`@wdio/visual-service` можна використовувати як звичайний сервіс. Ви можете налаштувати його у вашому конфігураційному файлі наступним чином:

```js
import path from "node:path";

// wdio.conf.ts
export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // Деякі опції, дивіться документацію для додаткових
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... більше опцій
            },
        ],
    ],
    // ...
};
```

Більше опцій сервісу можна знайти [тут](/docs/visual-testing/service-options).

Після налаштування в конфігурації WebdriverIO ви можете додавати візуальні перевірки до [ваших тестів](/docs/visual-testing/writing-tests).

### Capabilities
Щоб використовувати модуль візуального тестування, **вам не потрібно додавати якісь додаткові опції до ваших capabilities**. Проте, в деяких випадках вам може знадобитися додати додаткові метадані до ваших візуальних тестів, наприклад, `logName`.

`logName` дозволяє вам присвоїти власне ім'я кожному capability, яке потім може бути включене до імен файлів зображень. Це особливо корисно для розрізнення знімків екрана, зроблених у різних браузерах, пристроях або конфігураціях.

Щоб це зробити, ви можете визначити `logName` у розділі `capabilities` та переконатися, що опція `formatImageName` в сервісі візуального тестування посилається на нього. Ось як ви можете це налаштувати:

```js
import path from "node:path";

// wdio.conf.ts
export const config = {
    // ...
    // =====
    // Setup
    // =====
    capabilities: [
        {
            browserName: 'chrome',
            'wdio-ics:options': {
                logName: 'chrome-mac-15', // Власне ім'я логу для Chrome
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // Власне ім'я логу для Firefox
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // Деякі опції, дивіться документацію для додаткових
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // Формат нижче буде використовувати `logName` з capabilities
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... більше опцій
            },
        ],
    ],
    // ...
};
```

#### Як це працює
1. Налаштування `logName`:

    - У розділі `capabilities` призначте унікальний `logName` для кожного браузера чи пристрою. Наприклад, `chrome-mac-15` ідентифікує тести, що виконуються в Chrome на macOS версії 15.

2. Користувацьке іменування зображень:

    - Опція `formatImageName` інтегрує `logName` в імена файлів знімків. Наприклад, якщо `tag` — це homepage, а роздільна здатність — `1920x1080`, результуюче ім'я файлу може виглядати так:

        `homepage-chrome-mac-15-1920x1080.png`

3. Переваги користувацького іменування:

    - Розрізнення знімків з різних браузерів або пристроїв стає набагато простішим, особливо при керуванні базовими зображеннями та налагодженні розбіжностей.

4. Примітка щодо значень за замовчуванням:

    - Якщо `logName` не встановлено в capabilities, опція `formatImageName` відобразить його як порожній рядок в іменах файлів (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

Ми також підтримуємо [MultiRemote](https://webdriver.io/docs/multiremote/). Щоб це працювало правильно, переконайтеся, що ви додали `wdio-ics:options` до ваших
capabilities, як ви можете бачити нижче. Це забезпечить унікальне ім'я для кожного знімка екрана.

[Написання тестів](/docs/visual-testing/writing-tests) не відрізнятиметься порівняно з використанням [testrunner](https://webdriver.io/docs/testrunner)

```js
// wdio.conf.js
export const config = {
    capabilities: {
        chromeBrowserOne: {
            capabilities: {
                browserName: "chrome",
                "goog:chromeOptions": {
                    args: ["disable-infobars"],
                },
                // ЦЕ!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-one",
                },
            },
        },
        chromeBrowserTwo: {
            capabilities: {
                browserName: "chrome",
                "goog:chromeOptions": {
                    args: ["disable-infobars"],
                },
                // ЦЕ!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-two",
                },
            },
        },
    },
};
```

### Запуск програмно

Ось мінімальний приклад того, як використовувати `@wdio/visual-service` через опції `remote`:

```js
import { remote } from "webdriverio";
import VisualService from "@wdio/visual-service";

let visualService = new VisualService({
    autoSaveBaseline: true,
});

const browser = await remote({
    logLevel: "silent",
    capabilities: {
        browserName: "chrome",
    },
});

// "Запустіть" сервіс, щоб додати користувацькі команди до `browser`
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// або використовуйте це ЛИШЕ для збереження знімка екрана
await browser.saveFullPageScreen("examplePaged", {});

// або використовуйте це для перевірки. Обидва методи не потрібно комбінувати, див. FAQ
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### Переміщення по сайту за допомогою клавіші Tab

Ви можете перевірити, чи доступний веб-сайт, використовуючи клавішу <kbd>TAB</kbd>. Тестування цієї частини доступності завжди було трудомістким (ручним) завданням і досить складним для автоматизації.
За допомогою методів `saveTabbablePage` та `checkTabbablePage` ви тепер можете малювати лінії та точки на вашому веб-сайті, щоб перевірити порядок переміщення.

Майте на увазі, що це корисно лише для десктопних браузерів, а **НЕ\*\*** для мобільних пристроїв. Усі десктопні браузери підтримують цю функцію.

:::note

Робота натхненна блог-постом [Віва Річардса](https://github.com/vivrichards600) про ["АВТОМАТИЗАЦІЮ ПЕРЕМІЩЕННЯ ПО СТОРІНЦІ (ЧИ Є ТАКЕ СЛОВО?) З ВІЗУАЛЬНИМ ТЕСТУВАННЯМ"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript).

Спосіб вибору елементів, по яких можна переміщуватися, заснований на модулі [tabbable](https://github.com/davidtheclark/tabbable). Якщо є будь-які проблеми з переміщенням, перевірте [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) і особливо розділ [More Details](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details).

:::

#### Як це працює

Обидва методи створять елемент `canvas` на вашому веб-сайті та намалюють лінії та точки, щоб показати вам, куди перейде ваш TAB, якщо кінцевий користувач використовуватиме його. Після цього він створить знімок повної сторінки, щоб дати вам хороший огляд потоку.

:::important

**Використовуйте `saveTabbablePage` тільки коли вам потрібно створити знімок екрана і ви НЕ хочете порівнювати його **з **базовим** зображенням.\*\*\*\*

:::

Коли ви хочете порівняти потік переміщення з базовим, ви можете використати метод `checkTabbablePage`. Вам **НЕ** потрібно використовувати два методи разом. Якщо вже є базове зображення, яке може бути автоматично створене, якщо ви надали `autoSaveBaseline: true` при ініціалізації сервісу,
`checkTabbablePage` спочатку створить _фактичне_ зображення, а потім порівняє його з базовим.

##### Опції

Обидва методи використовують ті самі опції, що й [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) або
[`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage).

#### Приклад

Це приклад того, як працює переміщення на нашому [тестовому веб-сайті](https://guinea-pig.webdriver.io/image-compare.html):

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### Автоматичне оновлення невдалих візуальних знімків

Оновіть базові зображення через командний рядок, додавши аргумент `--update-visual-baseline`. Це:

-   автоматично скопіює знімок екрану, який зроблено, та помістить його в папку з базовими зображеннями
-   якщо є відмінності, тест буде пройдено, оскільки базове зображення було оновлено

**Використання:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

Під час виконання в режимі info/debug ви побачите наступні додані логи

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

## Підтримка Typescript

Цей модуль включає підтримку TypeScript, що дозволяє вам користуватися автозаповненням, безпекою типів та покращеним досвідом розробника при використанні сервісу візуального тестування.

### Крок 1: Додайте визначення типів
Щоб TypeScript розпізнавав типи модуля, додайте наступний запис до поля types у вашому tsconfig.json:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### Крок 2: Увімкніть безпеку типів для опцій сервісу
Щоб застосувати перевірку типів до опцій сервісу, оновіть вашу конфігурацію WebdriverIO:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// Імпортуйте визначення типу
import type { VisualServiceOptions } from '@wdio/visual-service';

export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // Опції сервісу
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // Забезпечує безпеку типів
        ],
    ],
    // ...
};
```

## Системні вимоги

### Версія 5 і вище

Для версії 5 і вище цей модуль є чисто JavaScript-модулем без додаткових системних залежностей, крім загальних [вимог проекту](/docs/gettingstarted#system-requirements). Він використовує [Jimp](https://github.com/jimp-dev/jimp), який є бібліотекою обробки зображень для Node, написаною повністю на JavaScript, без нативних залежностей.

### Версія 4 і нижче

Для версії 4 і нижче цей модуль покладається на [Canvas](https://github.com/Automattic/node-canvas), реалізацію canvas для Node.js. Canvas залежить від [Cairo](https://cairographics.org/).

#### Деталі встановлення

За замовчуванням, бінарні файли для macOS, Linux та Windows будуть завантажені під час `npm install` вашого проекту. Якщо у вас немає підтримуваної ОС або архітектури процесора, модуль буде скомпільовано на вашій системі. Це вимагає декількох залежностей, включаючи Cairo та Pango.

Для детальної інформації щодо встановлення див. [wiki node-canvas](https://github.com/Automattic/node-canvas/wiki/_pages). Нижче наведено одрядкові інструкції з встановлення для поширених операційних систем. Зауважте, що `libgif/giflib`, `librsvg` та `libjpeg` є необов'язковими і потрібні лише для підтримки GIF, SVG та JPEG відповідно. Потрібен Cairo v1.10.0 або пізніше.

<Tabs
defaultValue="osx"
values={[
{label: 'OS', value: 'osx'},
{label: 'Ubuntu', value: 'ubuntu'},
{label: 'Fedora', value: 'fedora'},
{label: 'Solaris', value: 'solaris'},
{label: 'OpenBSD', value: 'openbsd'},
{label: 'Window', value: 'windows'},
{label: 'Others', value: 'others'},
]}

> <TabItem value="osx">

     Використовуючи [Homebrew](https://brew.sh/):

     ```sh
     brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
     ```

    **Mac OS X v10.11+:** Якщо ви нещодавно оновилися до Mac OS X v10.11+ і маєте проблеми під час компіляції, виконайте наступну команду: `xcode-select --install`. Докладніше про проблему [на Stack Overflow](http://stackoverflow.com/a/32929012/148072).
    Якщо у вас встановлено Xcode 10.0 або вище, для збірки з джерел потрібен NPM 6.4.1 або вище.

</TabItem>
<TabItem value="ubuntu">

    ```sh
    sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
    ```

</TabItem>
<TabItem value="fedora">

    ```sh
    sudo yum install gcc-c++ cairo-devel pango-devel libjpeg-turbo-devel giflib-devel
    ```

</TabItem>
<TabItem value="solaris">

    ```sh
    pkgin install cairo pango pkg-config xproto renderproto kbproto xextproto
    ```

</TabItem>
<TabItem value="openbsd">

    ```sh
    doas pkg_add cairo pango png jpeg giflib
    ```

</TabItem>
<TabItem value="windows">

    Дивіться [wiki](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows)

</TabItem>
<TabItem value="others">

    Дивіться [wiki](https://github.com/Automattic/node-canvas/wiki)

</TabItem>
</Tabs>