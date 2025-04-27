---
id: visual-testing
title: Визуальное тестирование
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Что оно может делать?

WebdriverIO предоставляет сравнение изображений экранов, элементов или полной страницы для

-   🖥️ Настольных браузеров (Chrome / Firefox / Safari / Microsoft Edge)
-   📱 Мобильных / Планшетных браузеров (Chrome на эмуляторах Android / Safari на iOS-симуляторах / Симуляторах / реальных устройствах) через Appium
-   📱 Нативных приложений (эмуляторы Android / iOS-симуляторы / реальные устройства) через Appium (🌟 **НОВОЕ** 🌟)
-   📳 Гибридных приложений через Appium

через [`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service), который является легковесным сервисом WebdriverIO.

Это позволяет вам:

-   сохранять или сравнивать **экраны/элементы/полные страницы** с эталоном
-   автоматически **создавать эталон**, когда его нет
-   **блокировать пользовательские области** и даже **автоматически исключать** статус и/или панели инструментов (только для мобильных) во время сравнения
-   увеличивать размеры скриншотов элементов
-   **скрывать текст** во время сравнения веб-сайтов для:
    -   **улучшения стабильности** и предотвращения проблем с рендерингом шрифтов
    -   концентрации только на **макете** веб-сайта
-   использовать **различные методы сравнения** и набор **дополнительных матчеров** для лучшей читаемости тестов
-   проверять, как ваш веб-сайт **поддерживает навигацию с помощью клавиши Tab на клавиатуре)**, см. также [Навигация по сайту с помощью Tab](#tabbing-through-a-website)
-   и многое другое, смотрите опции [сервиса](./visual-testing/service-options) и [методов](./visual-testing/method-options)

Сервис представляет собой легковесный модуль для получения необходимых данных и скриншотов для всех браузеров/устройств. Мощь сравнения обеспечивается [ResembleJS](https://github.com/Huddle/Resemble.js). Если вы хотите сравнивать изображения онлайн, вы можете проверить [онлайн-инструмент](http://rsmbl.github.io/Resemble.js/).

:::info ПРИМЕЧАНИЕ для нативных/гибридных приложений
Методы `saveScreen`, `saveElement`, `checkScreen`, `checkElement` и матчеры `toMatchScreenSnapshot` и `toMatchElementSnapshot` могут использоваться для нативных приложений/контекста.

Пожалуйста, используйте свойство `isHybridApp:true` в настройках сервиса, если вы хотите использовать его для гибридных приложений.
:::

## Установка

Самый простой способ - сохранить `@wdio/visual-service` в качестве dev-зависимости в вашем `package.json`, через:

```sh
npm install --save-dev @wdio/visual-service
```

## Использование

`@wdio/visual-service` можно использовать как обычный сервис. Вы можете настроить его в вашем конфигурационном файле следующим образом:

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
                // Некоторые опции, подробнее в документации
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... дополнительные опции
            },
        ],
    ],
    // ...
};
```

Дополнительные опции сервиса можно найти [здесь](/docs/visual-testing/service-options).

После настройки в вашей конфигурации WebdriverIO, вы можете приступить к добавлению визуальных проверок в [ваши тесты](/docs/visual-testing/writing-tests).

### Capabilities
Для использования модуля визуального тестирования **вам не нужно добавлять никаких дополнительных опций к вашим capabilities**. Однако в некоторых случаях вы можете захотеть добавить дополнительные метаданные к своим визуальным тестам, такие как `logName`.

`logName` позволяет вам присвоить пользовательское имя каждому capability, которое затем может быть включено в имена файлов изображений. Это особенно полезно для различения скриншотов, сделанных в разных браузерах, устройствах или конфигурациях.

Чтобы включить это, вы можете определить `logName` в разделе `capabilities` и убедиться, что опция `formatImageName` в сервисе визуального тестирования ссылается на него. Вот как вы можете настроить это:

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
                logName: 'chrome-mac-15', // Пользовательское имя для Chrome
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // Пользовательское имя для Firefox
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // Некоторые опции, подробнее в документации
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // Формат ниже будет использовать `logName` из capabilities
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... дополнительные опции
            },
        ],
    ],
    // ...
};
```

#### Как это работает
1. Настройка `logName`:

    - В разделе `capabilities` назначьте уникальный `logName` каждому браузеру или устройству. Например, `chrome-mac-15` идентифицирует тесты, запущенные на Chrome на macOS версии 15.

2. Пользовательское именование изображений:

    - Опция `formatImageName` интегрирует `logName` в имена файлов скриншотов. Например, если `tag` - homepage, а разрешение - `1920x1080`, итоговое имя файла может выглядеть так:

        `homepage-chrome-mac-15-1920x1080.png`

3. Преимущества пользовательского именования:

    - Различение скриншотов из разных браузеров или устройств становится намного проще, особенно при управлении эталонами и отладке расхождений.

4. Примечание о значениях по умолчанию:

    - Если `logName` не установлен в capabilities, опция `formatImageName` будет отображать его как пустую строку в именах файлов (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

Мы также поддерживаем [MultiRemote](https://webdriver.io/docs/multiremote/). Для корректной работы убедитесь, что вы добавили `wdio-ics:options` к вашим
capabilities, как вы можете видеть ниже. Это обеспечит уникальное имя для каждого скриншота.

[Написание ваших тестов](/docs/visual-testing/writing-tests) не будет отличаться по сравнению с использованием [testrunner](https://webdriver.io/docs/testrunner)

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
                // ЭТО!!!
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
                // ЭТО!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-two",
                },
            },
        },
    },
};
```

### Запуск программно

Вот минимальный пример того, как использовать `@wdio/visual-service` через опции `remote`:

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

// "Запускаем" сервис, чтобы добавить пользовательские команды в `browser`
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// или используйте это ТОЛЬКО для сохранения скриншота
await browser.saveFullPageScreen("examplePaged", {});

// или используйте это для проверки. Оба метода не обязательно должны быть объединены, см. FAQ
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### Навигация по сайту с помощью Tab

Вы можете проверить, доступен ли веб-сайт с помощью клавиши <kbd>TAB</kbd> на клавиатуре. Тестирование этой части доступности всегда было трудоемкой (ручной) работой и довольно сложной для автоматизации.
С помощью методов `saveTabbablePage` и `checkTabbablePage` вы теперь можете рисовать линии и точки на вашем веб-сайте, чтобы проверить порядок табуляции.

Имейте в виду, что это полезно только для настольных браузеров и **НЕ** для мобильных устройств. Все настольные браузеры поддерживают эту функцию.

:::note

Работа вдохновлена постом в блоге [Viv Richards](https://github.com/vivrichards600) о ["АВТОМАТИЗАЦИИ ПРОВЕРКИ НАВИГАЦИИ ПО СТРАНИЦЕ С ПОМОЩЬЮ КЛАВИШИ TAB (ЭТО ВООБЩЕ СЛОВО?) С ПОМОЩЬЮ ВИЗУАЛЬНОГО ТЕСТИРОВАНИЯ"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript).

Способ выбора элементов для навигации основан на модуле [tabbable](https://github.com/davidtheclark/tabbable). Если есть какие-либо проблемы с табуляцией, проверьте [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) и особенно раздел [Дополнительные сведения](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details).

:::

#### Как это работает

Оба метода создадут элемент `canvas` на вашем веб-сайте и нарисуют линии и точки, чтобы показать вам, куда будет перемещаться ваш TAB, если конечный пользователь будет его использовать. После этого он создаст полностраничный скриншот, чтобы дать вам хороший обзор потока.

:::important

**Используйте `saveTabbablePage` только когда вам нужно создать скриншот и ВЫ НЕ хотите сравнивать его **с **эталонным** изображением.**

:::

Когда вы хотите сравнить поток табуляции с эталоном, вы можете использовать метод `checkTabbablePage`. Вы **НЕ** нуждаетесь в использовании двух методов вместе. Если уже есть эталонное изображение, которое может быть автоматически создано путем указания `autoSaveBaseline: true` при инициализации сервиса,
метод `checkTabbablePage` сначала создаст _текущее_ изображение, а затем сравнит его с эталоном.

##### Опции

Оба метода используют те же опции, что и [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) или
[`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage).

#### Пример

Вот пример того, как работает табуляция на нашем [тестовом веб-сайте](https://guinea-pig.webdriver.io/image-compare.html):

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### Автоматическое обновление неудачных визуальных снимков

Обновляйте эталонные изображения через командную строку, добавляя аргумент `--update-visual-baseline`. Это

-   автоматически скопирует фактически сделанный скриншот и поместит его в папку с эталонами
-   если есть различия, тест пройдет успешно, потому что эталон был обновлен

**Использование:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

При запуске с уровнем логирования info/debug вы увидите следующие добавленные логи

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

## Поддержка TypeScript

Этот модуль включает поддержку TypeScript, что позволяет вам пользоваться преимуществами автодополнения, типовой безопасности и улучшенного опыта разработчика при использовании сервиса визуального тестирования.

### Шаг 1: Добавление определений типов
Чтобы убедиться, что TypeScript распознает типы модуля, добавьте следующую запись в поле types в вашем tsconfig.json:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### Шаг 2: Включение типовой безопасности для опций сервиса
Чтобы обеспечить проверку типов для опций сервиса, обновите вашу конфигурацию WebdriverIO:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// Импортируйте определение типа
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
                // Опции сервиса
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // Обеспечивает типовую безопасность
        ],
    ],
    // ...
};
```

## Системные требования

### Версия 5 и выше

Для версии 5 и выше этот модуль является чисто JavaScript-модулем без дополнительных системных зависимостей, кроме общих [требований проекта](/docs/gettingstarted#system-requirements). Он использует [Jimp](https://github.com/jimp-dev/jimp) - библиотеку обработки изображений для Node, написанную полностью на JavaScript, без нативных зависимостей.

### Версия 4 и ниже

Для версии 4 и ниже этот модуль полагается на [Canvas](https://github.com/Automattic/node-canvas), реализацию canvas для Node.js. Canvas зависит от [Cairo](https://cairographics.org/).

#### Детали установки

По умолчанию, бинарные файлы для macOS, Linux и Windows будут загружены во время выполнения `npm install` вашего проекта. Если у вас нет поддерживаемой ОС или архитектуры процессора, модуль будет скомпилирован на вашей системе. Это требует нескольких зависимостей, включая Cairo и Pango.

Для получения подробной информации об установке см. [вики node-canvas](https://github.com/Automattic/node-canvas/wiki/_pages). Ниже приведены однострочные инструкции по установке для распространенных операционных систем. Обратите внимание, что `libgif/giflib`, `librsvg` и `libjpeg` являются необязательными и нужны только для поддержки GIF, SVG и JPEG соответственно. Требуется Cairo v1.10.0 или новее.

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

     Используя [Homebrew](https://brew.sh/):

     ```sh
     brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
     ```

    **Mac OS X v10.11+:** Если вы недавно обновились до Mac OS X v10.11+ и испытываете проблемы при компиляции, выполните следующую команду: `xcode-select --install`. Подробнее о проблеме [на Stack Overflow](http://stackoverflow.com/a/32929012/148072).
    Если у вас установлен Xcode 10.0 или выше, для сборки из исходников вам понадобится NPM 6.4.1 или выше.

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

    См. [вики](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows)

</TabItem>
<TabItem value="others">

    См. [вики](https://github.com/Automattic/node-canvas/wiki)

</TabItem>
</Tabs>